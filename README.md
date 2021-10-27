Список команд, выполненных на сервере при выполнении основной части:

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

platanus assemble -o Poil -f nd_paired1.fastq.trimmed nd_paired2.fastq.trimmed 2>assemble.log

platanus scaffold -o Poil -c Poil_contig.fa -IP1 nd_paired1.fastq.trimmed nd_paired2.fastq.trimmed -OP2 nd_mate1.fastq.int_trimmed nd_mate2.fastq.int_trimmed 2>scaffold.log

platanus gap_close  -o Poil -c Poil_scaffold.fa -IP1 nd_paired1.fastq.trimmed nd_paired2.fastq.trimmed -OP2 nd_mate1.fastq.int_trimmed nd_mate2.fastq.int_trimmed 2>gap_close.log

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

Статистика multiqc для исходных чтений:

![1 1](https://user-images.githubusercontent.com/72361668/139110692-059ccd9f-12a9-41cd-9f35-d86e1058c42f.png)

![1 2](https://user-images.githubusercontent.com/72361668/139110874-6c2cc64d-5cc9-4675-a77c-395d2eccc998.png)

![1 3 sequence quality](https://user-images.githubusercontent.com/72361668/139110884-372e7e3d-6550-4396-835e-df4838e42521.png)

![1 4 adapter content](https://user-images.githubusercontent.com/72361668/139110899-b9087a6a-1997-44bf-ad76-2f67eda2e2de.png)

Статистика multiqc для подрезанных чтений:

![2 1 General Statistics](https://user-images.githubusercontent.com/72361668/139111721-558b9cf0-b5c6-45ce-90a1-e635f6004d5b.png)

![2 2 Seq counts](https://user-images.githubusercontent.com/72361668/139111672-1ff395e5-03e5-49d1-a354-fc412842e970.png)

![2 3 Seq quality histo](https://user-images.githubusercontent.com/72361668/139111673-15425005-1d3a-4469-b463-f88b8bff2af0.png)

![2 4 adapter content](https://user-images.githubusercontent.com/72361668/139111676-2c0beadc-e1e7-4f80-beb6-7687ac33a51d.png)


Ссылки на google colab:

Анализ контигов: https://colab.research.google.com/drive/1atCTOuOQlgZ4-4gwlrkuKIpkpNdW5Qo7?usp=sharing

Анализ скаффолдов: https://colab.research.google.com/drive/1XY4wsrEVf1W4Q_X-EUPIhwKO3XlfryUV?usp=sharing

Необязательная часть: https://colab.research.google.com/drive/1YwaFygQz8ymITHd8i1HOlXGKtn_68KDN?usp=sharing

(количество чтений для сравнения: 2, 3, 4, 5 млн типа pair-end и соответствующие им 0.6, 0.9, 1.2, 1.5 млн mate-pairs)
