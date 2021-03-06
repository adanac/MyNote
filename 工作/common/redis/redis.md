##向redis放入数据
```
/**
     * 
     * 生成券号
     *  @param  id
     */
     public   void  toGenNoSingle(String  id )
    {
        CouponRuleDto  rule  = getCouponRule( id );
        String  ruleId =  rule .getId();
        
         int   num  =  rule .getSurplusNum();  //已剩余张数为准
        List<String>  list  =  new  ArrayList<String>();
         for ( int   i =0; i < num ; i ++)
        {
             //生成券号
            String  date  = DateUtils. format ( new  Date(), Constants. PATTREN_1 );
             //生成序列号
            String  no  =  date  + genSeq( i );
             list .add( no );
        }
        
         //
         int   count  = 1;
        
        List<String>  clist  =  new  ArrayList<String>();
         for (String  s : list )
        {
             clist .add( s );
             if  ( count  % 1000 == 0 ||  count  ==  list .size())
            {
                 redisClient .execute( clist , new  ShardedJedisPipelineAction<Object>(){
                      public  List<Object> doAction(ShardedJedisPipeline  pipeline ,Object  inParam ) {
                         List<String>  nlist = (ArrayList<String>)  inParam ;
                          for (String  s : nlist )
                         {
                             pipeline .rpush( ruleId ,  s );
                         }    
                          return   pipeline .syncAndReturnAll();
                    }
                });
                 clist  =  new  ArrayList<String>();
            }
             count ++;
        }
        
         //设置规则在redis中的过期时间
        Date  endDate  = DateUtils. parse ( rule .getEndTime(),Constants. PATTREN_2 );
        Date  now  =  new  Date();
         long   cha  = ( endDate .getTime()- now .getTime())/1000;
         redisClient .expire( ruleId , ( int ) cha );     }   

```
##多线程与redis的结合使用
```
/**
     * 
     * 生成券号
     *  @param  id
     */
     public   void  toGenNo(String  id )
    {
        CouponRuleDto  rule  = getCouponRule( id );
        String  ruleId =  rule .getId();
        
         int   num  =  rule .getIssuedNum();
         //条数很少，不需要创建线程即可
         if ( num < CRE_SIZE )
        {
            List<String>  list  =  new  ArrayList<String>();
             for ( int   i =0; i < num ; i ++)
            {
                 //生成券号
                String  date  = DateUtils. format ( new  Date(), Constants. PATTREN_1 );
                 //生成序列号
                String  no  =  date  + genSeq( i );
                 list .add( no );
            }
            
             redisClient .execute( list , new  ShardedJedisPipelineAction<Object>(){
                  public  List<Object> doAction(ShardedJedisPipeline  pipeline ,Object  inParam ) {
                     List<String>  nlist = (ArrayList<String>)  inParam ;
                      for (String  s : nlist )
                     {
                         pipeline .rpush( ruleId ,  s );
                     }    
                      return   pipeline .syncAndReturnAll();
                }
            });
             return ;
        }
        
         //创建多线程进行券号的生成
         int   totalPage  =0;
         if ( num % CRE_SIZE ==0){
             totalPage = num / CRE_SIZE ;
        }
         else
        {
             totalPage  = ( num / CRE_SIZE )+1;
        }
         log .info( "totalPage=" + totalPage );
        
        BlockingQueue<Runnable>  queue  =  new  LinkedBlockingQueue<Runnable>( totalPage );
        ExecutorService  service  =  new  ThreadPoolExecutor(10, 10, 0L,TimeUnit. MILLISECONDS ,  queue );
        
         for  ( int   i  = 1;  i  <=  totalPage ;  i ++) {
                
             final   int   startS  = ( i -1)* CRE_SIZE ;
             int   end  =  i * CRE_SIZE ;
             //判定是否最后一页
             if ( i == totalPage )
            {
                 end  =  num ;
            }
             final   int   endS  = end ;
        
            Runnable  run  =  new  Runnable() {
                 @Override
                 public   void  run() {
                    System. out .println( "-----------------startS=" + startS + ",endS=" + endS );
                    List<String>  list  =  new  ArrayList<String>();
                     for ( int   i = startS ; i < endS ; i ++)
                    {
                         //生成券号
                        String  date  = DateUtils. format ( new  Date(), Constants. PATTREN_1 );
                         //生成序列号
                        String  no  =  date  + genSeq( i );
                        System. out .println( "[" + i + "]---------------------" + no );
                         list .add( no );
                    }
                    setRedis( ruleId ,  list );
                }
                
                 private   void  setRedis(String  ruleId , List<String>  list ) {
                     //存入redis中去
                     redisClient .execute( list , new  ShardedJedisPipelineAction<Object>(){
                         public  List<Object> doAction(ShardedJedisPipeline  pipeline ,Object  inParam ) {
                             List<String>  nlist = (ArrayList<String>)  inParam ;
                            
                              for (String  s : nlist )
                             {
                                 pipeline .rpush( ruleId ,  s );
                             }    
                              return   pipeline .syncAndReturnAll();
                        }
                    });
                }
            };
             service .execute( run );
        }
         service .shutdown();     }   

```


##从redis中取数据
```
@Override
     public  CouponRuleDto getDetail(String  id )  throws  SysException {
         log .info( "getDetail id {}" , id );
        CouponRuleDto  dto  = null ;
         try
        {
             dto  =  new  RedisUtil( redisClient ).get(Constants. RULE_PRE + id , new  Cacheable<CouponRuleDto>(){
                 @Override
                 public  CouponRuleDto call() {
                    Map<String,Object>  paramMap  =  new  HashMap<String,Object>();
                     paramMap .put( "id" ,  id );
                     return   baseDao .queryForObject( "couponrule.select" ,  paramMap , CouponRuleDto. class );
                }
            });
             //log.info("dto {}",dto.toString());
        }
         catch (Exception  e )
        {
             log .error( "getCouponRule fail" , e );
             throw   new  SysException( "查询券规则详情失败" );
        }
         return   dto ;     }   

```

