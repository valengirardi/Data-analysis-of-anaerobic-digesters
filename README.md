# Data analysis of anaerobic digesters
The project aims to provide a comprehensive analysis of anaerobic digesters, fostering a deeper understanding of their performance through the synthesis of diverse data sets.

# Instrucciones de Instalación de QIIME 2 en MacOS
## Configuración del Entorno

```bash
# Setear el lugar en el que trabajo:
cd /Users/valentinagirardi/Downloads/Bioinf-enero

# Descargar Miniconda:
cp Miniconda3-latest-MacOSX-x86_64.sh ~/Miniconda3-latest-MacOSX-x86_64.sh

# Cambiar el directorio de inicio:
cd ~

# Dar permisos de ejecución:
chmod +x Miniconda3-latest-MacOSX-x86_64.sh

# Instalación:
./Miniconda3-latest-MacOSX-x86_64.sh -u -b -p $HOME/miniconda

# Iniciar Conda:
source ~/miniconda/etc/profile.d/conda.sh

# Activar Conda:
conda activate

# Versión compatible de conda:
conda install -n base conda=4.10
conda update -n base -c defaults conda

# Descargar el archivo de entorno específico de QIIME 2:
curl -sL \
  "https://data.qiime2.org/distro/core/qiime2-2020.8-py36-osx-conda.yml" > \
  "qiime2.yml"

# Crear el entorno desde el archivo:
conda env create -n qiime2 --file qiime2.yml

# Eliminar el archivo de configuración:
rm qiime2.yml

# Activar el nuevo entorno Conda:
conda activate qiime2

# Verificar la instalación:
Qiime info

