# Clasificación de células cervicales normales y patológicas en imágenes de la prueba de Papanicolaou para ayudar en la detección temprana de VPH - CM2D

El presente proyecto realiza la clasificación de células cervicales a partir de imágenes provenientes del frotis de Papanicolaou mediante visión artificial. El proyecto se desarrolló a partir del [set de datos](https://www.kaggle.com/datasets/prahladmehandiratta/cervical-cancer-largest-dataset-sipakmed): **SIPAKMED** proveniente de un trabajo de investigación de la Universidad de Ioannina, Grecia [1], el mismo nos provee de 4049 imágenes celulares anotadas por citopatólogos expertos. 
La clasificación se realiza en torno a cinco clases diferentes, dependiendo de la apariencia, morfología y basados además en las características visuales de las diferentes etapas del ciclo de vida de la célula cervical. Es así que se definen:
- Células normales clasificadas en dos categorias (**Superficial – Intermedio y Parabasal**)
- Células anormales no malignas clasificadas también en dos categorías (**Coilocitos y Disqueratósicas**)
- Células **Metaplásticas** (La presencia de este tipo de células en la prueba de Papanicolaou se asocia con mayores tasas de detección de lesiones precancerosas).

## Motivación
Bolivia tiene la tasa de cáncer de cuello uterino más alta en Latinoamérica según indicadores de la OMS, donde 26.3 por cada 100 mil mujeres mueren por esta causa y la tasa de incidencia más alta de América también es para nuestro país con 55.56 por cada 100 mil mujeres. Con el uso de pruebas como la del Papanicolau que es sencilla y rápida se puede detectar cambios en el cuello uterino antes de que se origine un cáncer o encontrar cáncer cervical cuando es pequeño y con más probabilidades de curar, en esta prueba se obtiene, mediante un ligero raspado, una muestra de células y de la mucosidad (moco) del exocérvix con una pequeña espátula o cepillo, finalmente, las muestras se examinan en el laboratorio. Una de las limitaciones de la prueba de Papanicolaou consiste en que los resultados necesitan ser examinados por el ojo humano, por lo que no siempre es posible un análisis preciso de cientos de miles de células en cada muestra.
El presente proyecto basa su proposición en los Objetivos de Desarrollo Sostenible (ODSs): 3 (Salud y bienestar), 5 (Igualdad de género) y 10 (Reducción de las desigualdades). Buscamos colaborar con la comunidad para visibilizar ésta problemática en un determinado sector de riesgo que predisponen a la población a tener un impacto social, económico y cultural negativo al tener algún tipo de alteración cervico-vaginal.

## Trabajos similares 
- Automated Pap Smear Cervical Cancer Screening Using Deep Learning
Presentado en 2019 en 41st Annual International Conference of the IEEE Engineering in Medicine and Biology Society (EMBC) por Sompawong, N., Mopan, J., Pooprasert, P., Himakhun, W., Suwannarurk, K., Ngamvirojcharoen, J., Tantibundhit, C. indica ser el primer intento de usar Mask R-CNN para detectar y analizar el núcleo de la célula cervical, detectando características nucleares normales y anormales. [2]

- DeepCervix: A deep learning-based framework for the classification of cervical cells using hybrid deep feature fusion techniques
Publicado por Elsevier en 2021 propone un ‘Hybird deep feature fusion’ denominado ‘DeepServix’ para la clasificación de células cervicales, en el proyecto se realiza pruebas con un número diferente de clases obteniendo en el parámetro ‘accuracy’:  99.85%, 99.38%, y 99.14% para 2-clases, 3 clases, y 5 clases respectivamente, en esta investigación se usó el mismo dataset que el que usamos en el presento proyecto [3]


## Capturas de pantalla
A continuación, se presentan capturas de pantalla de nuestra interfaz mostrando los resultados de inferencias ante imágenes de testeo:
![Deteccion 1](\img\captura1.png)
![Deteccion 2](\img\captura2.png)
![Deteccion 3](\img\captura3.png)
![Deteccion 4](\img\captura4.png)
![Deteccion 5](\img\captura5.png)


## Tecnologías/Frameworks utilizados
####Entrenamiento
- Google Colab Pro con una tarjeta gráfica NVIDIA Tesla P100 con 16GB de VRAM. 
  
####Despliegue
- Flask
- Docker

## Funcionalidades mas importantes
El proyecto se basará en el entrenamiento de un modelo del Estado del Arte **YOLOV5s** utilizado en la tarea de **Detección de Objetos**, con el dataset anteriormente descrito, ya que este alcanza muy buenos resultados bajo un buen rendimiento. Posteriormente este modelo entrenado es sintetizado y desplegado a través de una interfaz web que permitirá a un usuario ingresar una imagen del frotis de Papanicolau, realizar las inferencias en un servidor, devolver los resultados y mostrarlos al usuario.

## Instalación
####Entrenamiento
Todas las funcionalidades se incluyen directamente en los notebooks presentes en el repositorio:
- En primer lugar se utiliza el dataset proveniente de Kaggle **"SIPaKMeD database"**, donde se incluyen 966 imágenes de 2048x1536 además de 4049 imágenes recortadas con las detecciones realizadas con las células segmentadas. [1]
- Con el dataset se eliminan las carpetas innecesarias, además de las imágenes recortadas presentes en las carpetas "CROPPED". Las imágenes en formato ".bmp" y los datos en formato ".dat" deben estar presentes y distribuidas en las siguientes carpetas. **(dataset/im_Dyskeratotic, dataset/im_Koilocytotic, dataset/im_Metaplastic, dataset/im_Parabasal, dataset/im_Superficial-Intermediate)**
- Se utilizó un entorno en Google Colab para la ejecución del código, además de una carpeta compartida donde se adjuntó la carpeta comprimida del dataset mencionado. 
- El primer código en ejecutarse es el de **"conditioning_data_YOLOV5.ipynb"** ya que es necesario dar un formato a los datos obtenidos del dataset original. En este se cambia el formato de las imágenes de **".bmp"** a **".png"** (tomando un método de compresión sin pérdidas) además del redimensionamiento de las imágenes a **1000x1000**. Estos archivos se indexan y se generan las anotaciones correspondientes para YOLOV5. Además se generan nuevas carpetas con las imágenes y anotaciones, divididas en **"train" (20%), "valid" (15%), "test" (5%)**. Posteriormente se genera un archivo comprimido con estos datos mencionados.
- El segundo archivo en ejecutarse es **"train_YOLOV5.ipynb"** donde el mismo requiere la entrada del archivo generado en el anterior punto. El archivo descarga el código original para **YOLOV5** de GitHub, además de  la generación de los datos necesarios para su configuración y ejecución. Se toman en cuenta las siguientes clases: Superficial-Intermediate, Parabasal, Metaplastic, Koilocytotic, Dyskeratotic. Se utiliza un batch de 32 imágenes además de 279 épocas para su entrenamiento. Se utiliza el modelo de **YOLOV5s** ya que su arquitectura genera un balance entre el rendimiento y la precisión. Finalmente se genera un archivo comprimido con esta nueva carpeta de **YOLOV5**. 
- El tercer archivo en ejecutarse es **"test_YOLOV5.ipynb"** que solo requiere la entrada del dataset modificado, además de la última carpeta generada en el anterior punto (esta puede descargarse directamente en **yolov5_trained.zip**). Esta genera los resultados presentes en el el subset de "test", que tiene un total de 10 imágenes. Además de métricas tales como Precisión, Recall, F1 Score, Matriz de Confusión. Este código es también el encargado de generar la transformación a **Tensorflow JS** para su implementación en una página web.
####Despliegue
- El despliegue se realizará tomando en cuenta el framework Flask, para ejecutar el mismo será necesario ejecutar el archivo app.py (tomando en cuenta los requerimientos del archivo requirements.py)
- De ser necesario el despliegue en producción se tiene preparado el mismo vía Docker, con los requerimientos y las configuraciones de Docker correspondientes.

## Creditos
Maria Jose Huiza Capo 25%
Damaris Dayana Sarmiento Andrade 25%
Carlos Salvador Mendieta Villanueva 25%
Misael Mamani Quiroga 25%

## Referencias

[1] M. E. Plissiti, P. Dimitrakopoulos, G. Sfikas, C. Nikou, O. Krikoni, A. Charchanti, SIPAKMED: A new dataset for feature and image based classification of normal and pathological cervical cells in Pap smear images, IEEE International Conference on Image Processing (ICIP) 2018, Athens, Greece, 7-10 October 2018
[2] Sompawong N, Mopan J, Pooprasert P, Himakhun W, Suwannarurk K, Ngamvirojcharoen J, Vachiramon T, Tantibundhit C. Automated Pap Smear Cervical Cancer Screening Using Deep Learning. Annu Int Conf IEEE Eng Med Biol Soc. 2019 Jul;2019:7044-7048. doi: 10.1109/EMBC.2019.8856369. PMID: 31947460.
[3] Md Mamunur Rahaman, Chen Li, Yudong Yao, Frank Kulwa, Xiangchen Wu, Xiaoyan Li, and Qian Wang. 2021. DeepCervix: A deep learning-based framework for the classification of cervical cells using hybrid deep feature fusion techniques. Comput. Biol. Med. 136, C (Sep 2021). https://doi.org/10.1016/j.compbiomed.2021.104649
## Licencia

The MIT License

Copyright (c) 2022 CM2D

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.