FROM ubuntu:16.04

RUN  apt-get update && \
	apt-get install -y build-essential gfortran wget && \
	(unzip --version && echo "UNZIP INSTALLED") || (apt-get install unzip) && \ 
	(less --version && echo "LESS INSTALLED") || (apt-get install less) && \ 
	(nano --version && echo "NANO INSTALLED") || (apt-get install nano) && \ 
	(gawk --version && echo "AWK INSTALLED") || (apt-get install -y gawk)

CMD mkdir hapgen2 && \ 
	cd hapgen2 || echo "couldn't cd hapgen2" && \ 
	wget https://mathgen.stats.ox.ac.uk/genetics_software/hapgen/download/builds/x86_64/v2.2.0/hapgen2_x86_64.tar.gz && \
	tar zxvf hapgen2_x86_64.tar.gz && \ 
	wget https://mathgen.stats.ox.ac.uk/impute/hapmap3_r2_b36.tgz && \ 
	tar zxvf hapmap3_r2_b36.tgz && \ 
	gunzip hapmap3_r2_b36/hapmap3_r2_b36_chr6.haps.gz && \
	./hapgen2 -h hapmap3_r2_b36/hapmap3_r2_b36_chr6.haps -l hapmap3_r2_b36/hapmap3_r2_b36_chr6.legend -m hapmap3_r2_b36/genetic_map_chr6_combined_b36.txt -o OUTPUT -dl 2359212 1 2 2.5 -n 100 100 && \ 
	wget https://www.well.ox.ac.uk/~cfreeman/software/gwas/gtool_v0.7.5_x86_64.tgz && \ 
	tar zxvf gtool_v0.7.5_x86_64.tgz && \ 
	./gtool -M --g OUTPUT.cases.gen OUTPUT.controls.gen --s OUTPUT.cases.sample OUTPUT.controls.sample --og joined.gen --os joined.sample --threshold 0.9 --phenotype  pheno && \ 
	./gtool -G --g joined.gen --s joined.sample --ped joined.ped --map joined.map --threshold 0.9 --phenotype pheno && \ 
	head -n 100 joined.ped > first100.ped && \ 
	tail -n 100 joined.ped > last100.ped && \ 
	gawk '{split($0, a, FS, seps); a[6]="2"; for (i=1;i<=NF;i++) printf("%s%s", a[i], seps[i]); print ""}' first100.ped > phenofirst100.ped && \ 
	gawk '{split($0, a, FS, seps); a[6]="1"; for (i=1;i<=NF;i++) printf("%s%s", a[i], seps[i]); print ""}'  last100.ped > phenolast100.ped && \ 
	cat phenofirst100.ped phenolast100.ped > phenojoined.ped && \ 
	wget http://zzz.bwh.harvard.edu/plink/dist/plink-1.07-x86_64.zip && \ 
	unzip plink-1.07-x86_64.zip && \ 
	cd plink-1.07-x86_64 && \ 
	./plink --noweb --ped ../phenojoined.ped --map ../joined.map --assoc --allow-no-sex --out Q && \ 
	cd .. && \ 
	wget http://www.well.ox.ac.uk/~gav/resources/snptest_v2.5.4-beta3_linux_x86_64_static.tgz && \ 
	tar zxvf snptest_v2.5.4-beta3_linux_x86_64_static.tgz && \ 
	snptest_v2.5.4-beta3_linux_x86_64_static/snptest_v2.5.4-beta3  -data joined.gen joined.sample -frequentist 1 -method score -o SNP -pheno pheno
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	