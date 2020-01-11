# mat2tecplot 4.0
将m*n矩阵转化为tecplot数据格式，输入格式比如“./mat2tecplot.sh -2000 2000 -500 500 c_keatom test.txt”，输出文件为输入文件加后缀".tecplot.dat",其中mat2tecplot.sh文件内容如下：
```
#! /bin/bash

if [ ! $# == 6 ]; then
echo "Need parameters! Usage: $0 Xmin Xmax Ymin Ymax Label input_file"
exit
fi

xmin="$1"
xmax="$2"
ymin="$3"
ymax="$4"
colorlabel="$5"
inputfile="$6"
outputfile=$inputfile.tp
tmpout=$outputfile.tmp

M=`cat $inputfile|grep -v "#"|wc -l`                   # 统计列数
N=`head -2 $inputfile|tail -1| awk '{print NF}'`       # 统计行数

dY=`echo |awk '{print ('$ymax'-1.0*'$ymin')/('$M'-1.0)}'`
dX=`echo |awk '{print ('$xmax'-1.0*'$xmin')/('$N'-1.0)}'`

#echo "M=$M N=$N dX=$dX dY=$dY"
#sleep 20

rm -rf $outputfile $tmpout

MAT=`cat $inputfile|grep -v "#"`

echo "TITLE = \"Tecplot Data Format\"" >> $outputfile
echo "VARIABLES = \"X\", \"Y\", \"$colorlabel\"" >> $outputfile
echo "ZONE I=$M, J=$N, F=POINT" >> $outputfile

i=1;

for k in $MAT
do
	m=$[($i-1)/$N+1];
	n=$[$i-$m*$N+$N];
	echo "$n $m $k" >> $tmpout
	i=$[$i+1];
done

awk -v a0="$xmin" -v a1="$dX" -v b0="$ymin" -v b1="$dY" '{print a0+($1-1)*a1, b0+($2-1)*b1, $3}' $tmpout >> $outputfile
rm -rf $tmpout
```

# mat2tecplot 3.0
将m*n矩阵转化为tecplot数据格式，输入格式比如“./mat2tecplot.sh -2000 2000 -500 500 c_keatom test.txt”，输出文件为输入文件加后缀".tecplot.dat",其中mat2tecplot.sh文件内容如下：
```
#! /bin/bash

if [ ! $# == 6 ]; then
echo "Need parameters! Usage: $0 Xmin Xmax Ymin Ymax Label input_file"
exit
fi

xmin="$1"
xmax="$2"
ymin="$3"
ymax="$4"
colorlabel="$5"
inputfile="$6"
outputfile=$inputfile.tecplot.dat

M=`cat $inputfile|wc -l`
N=`head -1 $inputfile | awk '{print NF}'`

rm -rf $outputfile

MAT=`cat $inputfile`

echo "TITLE = Tecplot Data Format" >> $outputfile
echo "VARIABLES = "X", "Y", "$colorlabel"" >> $outputfile
echo "ZONE I=$M, J=$N, F=POINT" >> $outputfile

for ((i=1;i<$M+1;i++))
{
   for ((j=1;j<$N+1;j++))
   {
      data=$(awk 'NR=='$i'{print $'$j'}' $inputfile)
      yy=$[$ymin+($i-1)*($ymax-$ymin)/($M-1)|bc]
      xx=$[$xmin+($j-1)*($xmax-$xmin)/($N-1)|bc]
      echo "$xx,$yy,$data" >> $outputfile
   }
}
```
# mat2tecplot 2.0
将m*n矩阵转化为tecplot数据格式，输入格式比如“./mat2tecplot.sh 200 200 -2000 2000 -500 500 c_keatom test.txt test.dat”，其中mat2tecplot.sh文件内容如下：
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
      yy=$[$ymin+($i-1)*($ymax-$ymin)/($M-1)|bc]
      xx=$[$xmin+($j-1)*($xmax-$xmin)/($N-1)|bc]
      echo "$xx,$yy,$data" >> $outputfile
   }
}
```
# mat2tecplot 1.0
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
