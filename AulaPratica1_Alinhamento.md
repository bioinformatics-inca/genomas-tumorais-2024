# Aula 03 - Variantes Genômicas - Liliane Conteville

## Navegação

``ls``
Lista os arquivos e diretórios no diretório atual.

``cd Documentos``
Acessa o diretório chamado Documentos.

``ls``
Lista os arquivos e diretórios no diretório atual.

``cd disciplina``
Acessa o diretório chamado disciplina.

## Visualização de arquivo FASTA

``ls ref/``
Lista os arquivos contidos no diretório ref.

``head ref/chr7.fa``
Imprime as primeiras linhas do arquivo chr7.fa que está no diretório ref.

``less ref/chr7.fa``
Abre o arquivo chr7.fa em modo paginado.

## Manipulação de dados FASTQ

``ls``
Exibe os arquivos no diretório atual.

``ls dados/``
Lista os arquivos no diretório dados.

``zcat dados/COLO829T.R1.fastq.gz | head``
Exibe as primeiras linhas do arquivo COLO829T.R1.fastq.gz sem decomprimir.

``zcat dados/COLO829T.R1.fastq.gz | grep "^@"``
Filtra as linhas que começam com @.

``zcat dados/COLO829T.R1.fastq.gz | grep -c "^@"``
Conta quantas linhas começam com @.

## Qualidade de sequências
``mkdir quali_raw``
Cria o diretório quali_raw para armazenar os resultados.

``fastqc dados/COLO829T.R1.fastq.gz --outdir quali_raw/``
Analisa a qualidade do arquivo COLO829T.R1.fastq.gz usando o FastQC e salva os resultados em quali_raw.

``ls quali_raw/``
Lista os arquivos gerados no diretório quali_raw.

``firefox quali_raw/COLO829T.R1_fastqc.html``
Abre o resultado do FastQC no navegador Firefox.

## Filtragem e Trimagem
``mkdir trimagem``
Cria o diretório trimagem para armazenar os resultados.

``java -jar trimmomatic-0.39.jar -h``
Exibe a ajuda do Trimmomatic.

``java -jar trimmomatic-0.39.jar PE -phred33 dados/COLO829T.R1.fastq.gz dados/COLO829T.R2.fastq.gz trimagem/COLO829T.R1_paired.fastq trimagem/COLO829T.R1_unpaired.fastq trimagem/COLO829T.R2_paired.fastq trimagem/COLO829T.R2_unpaired.fastq ILLUMINACLIP:ref/adapters.fa:2:30:10``
Realiza a trimagem das sequências para remover adaptadores e bases de baixa qualidade.

##  Qualidade de sequências II
``mkdir quali_trim``
Cria o diretório quali_trim para os resultados da qualidade pós-trimagem.

``fastqc trimagem/COLO829T.R1_paired.fastq --outdir quali_trim/``
Avalia a qualidade do arquivo COLO829T.R1_paired.fastq após a trimagem.

``ls quali_trim/``
Lista os arquivos gerados em quali_trim.

``firefox quali_trim/amostraX_R1_paired_fastqc.html``
Abre o resultado do FastQC no navegador Firefox.

##  Alinhamento
``ls ref/``
Lista os arquivos no diretório ref.

``bwa index ref/chr7.fa``
Indexa o arquivo de referência chr7.fa para alinhamento com o BWA.

``ls ref/``
Confirma a criação de arquivos no diretório ref.

``mkdir alinhamento/``
Cria o diretório alinhamento para os resultados do alinhamento.

``bwa mem ref/chr7.fa -R '@RG\tID:COLO829\tSM:COLO829T\tLB:COLO829\tPL:ILLUMINA' data/COLO829T.R1.fastq.gz data/COLO829T.R2.fastq.gz > alinhamento/COLO829T.sam``
Realiza o alinhamento dos arquivos de leitura R1 e R2 contra o genoma de referência. A opção "-R" adiciona informações sobre o grupo de leitura (read group).

``less alinhamento/COLO829T.sam``
Abre o arquivo de alinhamento gerado para inspeção.

## Manipulação de Arquivos BAM
``samtools view -b alinhamento/COLO829T.sam > alinhamento/COLO829T.bam``
Converte o arquivo SAM para o formato BAM (binário).

``samtools sort alinhamento/COLO829T.bam -o alinhamento/COLO829T.sorted.bam``
Ordena o arquivo BAM para análises subsequentes.

``samtools index alinhamento/COLO829T.sorted.bam``
Cria um índice para o arquivo BAM ordenado.

## Visualização do Alinhamento

igv

gi|568336017|gb|CM000669.2|:79,679,930-79,679,980

gi|568336017|gb|CM000669.2|:79,757,420-79,757,521

gi|568336017|gb|CM000669.2|:79,902,077-79,902,127

gi|568336017|gb|CM000669.2|:79,856,089-79,856,497

gi|568336017|gb|CM000669.2|:80,431,635-80,433,270

gi|568336017|gb|CM000669.2|:80,454,505-80,454,606


