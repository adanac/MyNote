##读取文件
```


1.读取一个txt文件,方法很多种我使用了字符流来读取（为了方便）

 

  FileReader fr = new FileReader("f:\\TestJava. Java ");
   BufferedReader bf = new BufferedReader(fr);

//这里进行读取

int b;
   while((b=bf.read())!=-1){
    System.out.println(bf.readLine());
   }

发现每行的第一个字符都没有显示出来，原因呢：b=bf.read())!=-1  每次都会先读取一个字节出来，所以后面的bf.readLine());
读取的就是每行少一个字节

所以，应该使用

String valueString = null;
   while ((valueString=bf.readLine())!=null){
    
    
    System.out.println(valueString);
   }
```