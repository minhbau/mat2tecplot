# mat2tecplot
将m*n矩阵转化为tecplot数据格式
```
   #! /bin/bash

   M=200
   N=200

   inputfile=test.txt

   outputfile=test.dat

   rm -rf $outputfile

   echo "TITLE = Tecplot Data Format" >> $outputfile

   echo "VARIABLES = "X", "Y", "c_keatom"" >> $outputfile

   echo "ZONE I=$M, J=$N, F=POINT" >> $outputfile

   for ((i=1;i<$M+1;i++))
   {
      for ((j=1;j<$N+1;j++))  
      {   
       data=$(awk 'NR=='$i'{print $'$j'}' $inputfile)      
       printf "$j,$i,$data\n" >> $outputfile      
      }   
   }
```
