## 01_basic_stats

# Establecer el directorio de trabajo:
cd /Users/valentinagirardi/Downloads/Bioinf-enero

# Crear directorio para estadísticas básicas:
mkdir 01_basic_stats
OUTPUT_DIR=01_basic_stats
OUTPUT=${OUTPUT_DIR}/stats.txt
TOTAL=0

# Contar el total de secuencias para cada muestra y el total de pares:
for FILE in data/*.fastq
do
    NUM=$(( $(cat "${FILE}" | wc -l) / 4 ))
    TOTAL=$(( TOTAL + NUM ))
    echo "${FILE}: ${NUM}" >> ${OUTPUT}
done

echo "TOTAL DE PARES: ${TOTAL}" >> ${OUTPUT}

## 02_raw_data_qc

# Establecer directorios de entrada y salida:
INPUT_DIR=/Users/valentinagirardi/Downloads/Bioinf-enero/data
OUTPUT_DIR=/Users/valentinagirardi/Downloads/Bioinf-enero/02_raw_data_qc

# Crear directorio de salida:
mkdir ${OUTPUT_DIR}

# Realizar análisis de calidad con FastQC para cada muestra:
fastqc "${INPUT_DIR}"/*.fastq -o "${OUTPUT_DIR}/"

# Generar informe MultiQC:
cd "${OUTPUT_DIR}/"
multiqc .

## 03_metadata

# Establecer el directorio de trabajo:
cd /Users/valentinagirardi/Downloads/Bioinf-enero/data

# Crear archivo de manifiesto:
echo -e "id-muestra,ruta-absoluta,direccion" > /Users/valentinagirardi/Downloads/Bioinf-enero/data/manifest.csv

# Generar metadatos para cada muestra:
for file1 in /Users/valentinagirardi/Downloads/Bioinf-enero/data/*_1.fastq;
do
    file2="${file1/_1.fastq/_2.fastq}"

    id=$(basename "$file1" | cut -d '_' -f 1-2)
    ruta_absoluta1="$file1"
    ruta_absoluta2="$file2"

    echo -e "$id,$ruta_absoluta1,forward" >> /Users/valentinagirardi/Downloads/Bioinf-enero/data/manifest.csv
    echo -e "$id,$ruta_absoluta2,reverse" >> /Users/valentinagirardi/Downloads/Bioinf-enero/data/manifest.csv
done
