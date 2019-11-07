# mat2tecplot1.0
将m*n矩阵转化为tecplot数据格式，输入格式比如“./matrix-tecplot.sh 200 200 -2000 2000 -500 500 c_keatom test.txt test.dat”
```
#! /bin/bash

if [ ! $# == 9 ]; then
echo "Need parameters! Usage: $0 number_of_row number_of_column min_of_x max_of_x min_of_y max_of_y colorlabel input_file output_file"
exit
fi

M="$1"
N="$2"
xmin="$3"
xmax="$4"
ymin="$5"
ymax="$6"
colorlabel="$7"
inputfile="$8"
outputfile="$9"

rm -rf $outputfile

echo "TITLE = Tecplot Data Format" >> $outputfile
echo "VARIABLES = "X", "Y", "$colorlabel"" >> $outputfile
echo "ZONE I=$M, J=$N, F=POINT" >> $outputfile

for ((i=1;i<$M+1;i++))
{
   for ((j=1;j<$N+1;j++))
   {
      data=$(awk 'NR=='$i'{print $'$j'}' $inputfile)
      yy=$[$ymin+($i-1)*($ymax-$ymin)/($N-1)|bc]
      xx=$[$xmin+($j-1)*($xmax-$xmin)/($M-1)|bc]
      echo "$xx,$yy,$data" >> $outputfile
   }
}
```
