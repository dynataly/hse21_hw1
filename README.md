Список команд, выполненных на сервере:

bash
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
seqtk sample -s1704 oil_R1.fastq 5000000 > nd_paired1.fastq
seqtk sample -s1704 oil_R2.fastq 5000000 > nd_paired2.fastq
fastqc nd_paired1.fastq
fastqc nd_paired2.fastq
seqtk sample -s1704 oilMP_S4_L001_R1_001.fastq 1500000 > nd_mate1.fastq
seqtk sample -s1704 oilMP_S4_L001_R2_001.fastq 1500000 > nd_mate2.fastq
fastqc nd_mate1.fastq
fastqc nd_mate2.fastq
multiqc .
platanus_trim nd_paired1.fastq nd_paired2.fastq
platanus_internal_trim nd_mate1.fastq nd_mate2.fastq
rm nd_paired1.fastq
rm nd_paired2.fastq
rm nd_mate1.fastq
rm nd_mate2.fastq

#platanus:
platanus assemble -o Poil -f nd_paired1.fastaq.trimmed nd_paired2.fastaq.trimmed 2>assemble.log

platanus scaffold -o Poil -c Poil_contig.fa -IP1 nd_paired1.fastaq.trimmed nd_paired2.fastaq.trimmed -OP2 nd_mate1.fastq.int_trimmed nd_mate2.fastq.int_trimmed 2>scaffold.log

platanus gap_close  -o Poil -c Poil_scaffold.fa -IP1 nd_paired1.fastaq.trimmed nd_paired2.fastaq.trimmed -OP2 nd_mate1.fastq.int_trimmed nd_mate2.fastq.int_trimmed 2>gap_close.log

fastqc nd_paired1.fastq.trimmed
fastqc nd_paired2.fastq.trimmed
fastqc nd_mate1.fastq.int_trimmed
fastqc nd_mate2.fastq.int_trimmed

mkdir quality
mv-v *html quality/
mv -v *zip quality/
mv -v multi* quality/
mkdir platanus_data
mv -v Poil* platanus_data/
mv -v *log platanus_data/
mkdir original_data
mv -v *fastq original_data/
rm *trimmed


Статистика multiqc:

Ссылки на google colab:

Анализ контигов: https://colab.research.google.com/drive/1atCTOuOQlgZ4-4gwlrkuKIpkpNdW5Qo7?usp=sharing


