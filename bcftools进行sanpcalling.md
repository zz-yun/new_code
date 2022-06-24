#samtools+bcftools进行sanpcalling
for id in `ls *.bam`
do
	echo "bcftools mpileup -Ou -f /dellfcsan/genomes/plants/Citrus/citrus_maxima/pummelo/v1.0/grapefruit_v1.fa $id | bcftools call -mv -Oz > ./$id.vcf.gz ">>call_variation.txt
done

#tabix建立压缩后vcf索引
#!/bin/bash

for file in `ls *.gz`
do
     `echo tabix -p vcf $file `
done

#合并压缩后的vcf
bcftools merge 10-1A.bam.vcf.gz 10-2A.bam.vcf.gz 11-1A.bam.vcf.gz 11-2A.bam.vcf.gz 1-1A.bam.vcf.gz 12-1A.bam.vcf.gz 12-2A.bam.vcf.gz 1-2A.bam.vcf.gz 13-1A.bam.vcf.gz 13-2A.bam.vcf.gz 14-1A.bam.vcf.gz 14-2A.bam.vcf.gz 15-1A.bam.vcf.gz 15-2A.bam.vcf.gz 16-1A.bam.vcf.gz 16-2A.bam.vcf.gz 17-1A.bam.vcf.gz 17-2A.bam.vcf.gz 18-1A.bam.vcf.gz 18-2A.bam.vcf.gz 19-1A.bam.vcf.gz 19-2A.bam.vcf.gz 20-1A.bam.vcf.gz 20-2A.bam.vcf.gz 21-1A.bam.vcf.gz 21-2A.bam.vcf.gz 2-1A.bam.vcf.gz 22-1A.bam.vcf.gz 22-2A.bam.vcf.gz 2-2A.bam.vcf.gz 23-1A.bam.vcf.gz 23-2A.bam.vcf.gz 24-1A.bam.vcf.gz 24-2A.bam.vcf.gz 25-1A.bam.vcf.gz 25-2A.bam.vcf.gz 26-1A.bam.vcf.gz 26-2A.bam.vcf.gz 27-1A.bam.vcf.gz 27-2A.bam.vcf.gz 28-1A.bam.vcf.gz 28-2A.bam.vcf.gz 29-1A.bam.vcf.gz 29-2A.bam.vcf.gz 30-1A.bam.vcf.gz 30-2A.bam.vcf.gz 3-1A.bam.vcf.gz 3-2A.bam.vcf.gz 4-1A.bam.vcf.gz 4-2A.bam.vcf.gz 5-1A.bam.vcf.gz 5-2A.bam.vcf.gz 6-1A.bam.vcf.gz 6-2A.bam.vcf.gz 7-1A.bam.vcf.gz 7-2A.bam.vcf.gz 8-1A.bam.vcf.gz 8-2A.bam.vcf.gz 9-1A.bam.vcf.gz 9-2A.bam.vcf.gz mandarin-1A.bam.vcf.gz mandarin-2A.bam.vcf.gz pomelo-1A.bam.vcf.gz pomelo_2A.bam.vcf.gz -Oz -o 64samples.vcf.gz
#vcf过滤
bcftools filter -i'%QUAL>20' calls.vcf.gz
#vcf文件过滤(二等位，平均depth>=10,nomissing,MAF0.1-0.5,HWE<avg)
vcftools --vcf 64samples.vcf  --min-meanDP 10 --min-alleles 2 --max-alleles 2 --max-missing 1  --remove-indels --maf 0.1 --max-maf 0.5  --recode --recode-INFO-all --out 64_miss0_maf0.1_0.5_SNP_filted.vcf


