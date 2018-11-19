杀掉tomcat进程后仍然端口占用的脚本
--------------

		#!/bin/bash
		#source /etc/profile
		tomcat_home=/soft/tomcat7/bin
		port=6789

		function getPid(){
			#pid=`ps -ef|grep -v grep|grep ${tomcat_home}|awk '{print $2}'`
			pid=`ps -ef|grep -v grep|grep ${tomcat_home} |awk '{print $2}'`	
		}
		getPid
		echo 'pid: ' $pid
		if [ $pid ];then
			$tomcat_home/shutdown.sh
		fi
		sleep 1
		getPid
		if [ $pid ];then
			kill -9 $pid
		fi
		echo ' gateway restart...'
		netstat_status=true
		client_num=`netstat -anp |grep $port|wc -l`
		client_open_num=$client_num
		echo `date` '   not close client :' $client_num
		while $netstat_status
		do
			net_connection_count=`netstat -anp |grep $port|wc -l`
			if [[ $net_connection_count -lt $client_num ]] && 
							[[ $net_connection_count -ne $client_open_num ]];then
				
				client_open_num=$net_connection_count
		                echo `date` '   not close client :' $net_connection_count
		        fi
			if [ $net_connection_count -eq 0 ];then
		        	netstat_status=false
			fi
		done
		$tomcat_home/startup.sh
		#echo 'tomcat-gateway start success!'


在指定目录下jar包内搜索文件
--------------

		#!/bin/bash
		# sh class_file [path] file_name 
		file_name=''
		if [ $# -gt 1 ];then
			 cd $1
			 file_name=$2
		else
			 file_name=$1
		fi
		find $path -name "*.jar" -type f > /tmp/jar.txt
		for item in $(cat /tmp/jar.txt)
		do 
		    jar -tvf $item |grep  $file_name
		    if [ $? -eq 0 ]; then
		       echo "the jar file is $item"
		    fi
		done
