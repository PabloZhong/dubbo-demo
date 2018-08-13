FROM docker.io/busybox:latest 


ADD target/dubbo-demo-consumer-2.5.7-assembly.tar.gz .

RUN mv dubbo-demo-consumer-2.5.7/ dubbo-demo-consumer/


##ADD target/dubbo-demo-provider-2.5.7-assembly.tar.gz /dubbo

COPY start-docker.sh dubbo-demo-consumer/bin/
CMD "tail" "-f" "/dev/null"

##CMD ["sh","-c","/dubbo/dubbo-demo-consumer-2.5.7/bin/start-docker.sh"]


#FROM docker.io/busybox:latest 
#RUN mkdir app
#ADD /target/dubbo-demo-consumer-2.5.7-assembly.tar.gz .

#RUN cp -rf dubbo-demo-consumer-2.5.7/bin/ /app 

#RUN cp -rf dubbo-demo-consumer-2.5.7/conf/ /app 
#RUN cp -rf dubbo-demo-consumer-2.5.7/lib/ /app 
#COPY start-docker.sh /app/bin 

#RUN chmod -R 777 /app/bin/
#CMD "tail" "-f" "/dev/null"
