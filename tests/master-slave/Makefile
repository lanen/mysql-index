
init:
	mkdir -p master/data ; \
	mkdir -p master/conf ; \
	mkdir -p slave1/data ; \
	mkdir -p slave1/conf ; \
	mkdir -p slave2/data ; \
	mkdir -p slave2/conf ; \
	cat config/template | sed "s/_n_/jy-mysql-master/" > master/start; \
	sed -i '' 's/_p_/3316/' master/start ; \
	cat config/template | sed "s/_n_/jy-mysql-slave1/" > slave1/start; \
	sed -i '' 's/_p_/3326/' slave1/start ; \
	cat config/template | sed "s/_n_/jy-mysql-slave2/" > slave2/start; \
	sed -i '' 's/_p_/3336/' slave2/start ; \
	cat config/cnf_master | sed "s/_s_/100/" > master/conf/my.cnf; \
	cat config/cnf_slave | sed "s/_s_/201/" > slave1/conf/my.cnf; \
	cat config/cnf_slave | sed "s/_s_/202/" > slave2/conf/my.cnf; \
	sed -i '' 's/_ip_/jy-mysql-master/' slave1/conf/my.cnf ; \
	sed -i '' 's/_p_/3316/' slave1/conf/my.cnf ; \
	sed -i '' 's/_ip_/jy-mysql-master/' slave2/conf/my.cnf ; \
	sed -i '' 's/_p_/3316/' slave2/conf/my.cnf ; \
	sed -i '' 's/_db_/article/' slave1/conf/my.cnf ; \
	sed -i '' 's/_db_/article/' slave2/conf/my.cnf ; \
	chmod +x master/start; \
	chmod +x slave1/start; \
	chmod +x slave2/start; 

clean:
	docker rm jy-mysql-master; \
	docker rm jy-mysql-slave1; \
	docker rm jy-mysql-slave2; \
	rm -rf master; \
	rm -rf slave1; \
	rm -rf slave2; 

stop:
	docker stop jy-mysql-master; \
	docker stop jy-mysql-slave1; \
	docker stop jy-mysql-slave2; 
	
start:
	./start

master:
	
