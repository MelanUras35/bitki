
pip install tensorflow==2.14.0
pip install keras
pip install PyQt5
pip install PyQt5-tools
pip install PyQt5Designer

from keras.models import load_model  # TensorFlow is required for Keras to work
from PIL import Image, ImageOps  # Install pillow instead of PIL
import numpy as np

from flask import Flask
from flask_ngrok import run_with_ngrok

app = Flask(__name__)
run_with_ngrok(app)  # Ngrok ile Flask'ı başlatır

@app.route("/")
def home():
    return "<h1>Merhaba bu bitki bulucu!</h1>"

app.run()

import sys
from PyQt5.QtCore import *
from PyQt5.QtGui import *
from PyQt5.QtWidgets import *



class window(QWidget):
    def _init_(self, parent = None):
      super(window, self)._init_(parent)
      self.resize(200,50)
      self.setWindowTitle("Bitki Bulucu")
      self.yazi = QLabel(self)
      self.yazi.setText("bitki bulucu")
      Font = QFont()
      font.setFamily("arial")
      font.setPointSize(16)
      self.yazi.setFont(font)
      self.yazi.move(50,20)

def main():
    app = QApplication(sys.argv)
    ekran = window()
    ekran.show()
    sys.exit(app.exec_())

if _name_ == "_main_":
    pass

def bitkibulucu(imageadi):
  np.set_printoptions(suppress=True)

# Load the model
  model = load_model("keras_model.h5", compile=False)

  # Load the labels
  class_names = open("labels.txt", "r").readlines()

  # Create the array of the right shape to feed into the keras model
  # The 'length' or number of images you can put into the array is
  # determined by the first position in the shape tuple, in this case 1
  data = np.ndarray(shape=(1, 224, 224, 3), dtype=np.float32)

  # Replace this with the path to your image
  image = Image.open(imageadi).convert("RGB")

  # resizing the image to be at least 224x224 and then cropping from the center
  size = (224, 224)
  image = ImageOps.fit(image, size, Image.Resampling.LANCZOS)

  # turn the image into a numpy array
  image_array = np.asarray(image)

  # Normalize the image
  normalized_image_array = (image_array.astype(np.float32) / 127.5) - 1

    # Load the image into the array
  data[0] = normalized_image_array

  # Predicts the model
  prediction = model.predict(data)
  index = np.argmax(prediction)
  class_name = class_names[index]
  confidence_score = prediction[0][index]

    # Print prediction and confidence score
  print("Class:", class_name[2:], end="")
  print("Confidence Score:", confidence_score)
  if (class_name[2:].strip() == "Adaçayı"):
    print ("Adaçayı güneşli ve sıcak yerleri sever, bu yüzden bol güneş alan bir konumda yetiştirilmesi idealdir. Toprağı iyi drene edilmiş, hafif kireçli ve pH değeri nötr veya hafif alkali olmalıdır. Tohumları ekmeden önce toprağı iyice hazırlayın ve hafif nemli tutun. İlkbaharda veya sonbaharda ekebilirsiniz. Bitki büyüdükçe budayarak daha sık ve gür büyümesini sağlayabilirsiniz")
  if (class_name[2:].strip()=="Fesleğen"):
    print ("Fesleğen, sıcak ve güneşli bir ortamda en iyi şekilde büyür. Toprağı nemli ve besin bakımından zengin olmalıdır. Tohumları ilkbaharda doğrudan dışarıya ya da içeride saksılara ekebilirsiniz. Çimlenme için 18-24 derece arasında bir sıcaklık idealdir. Bitkiyi düzenli olarak sulayın, ancak su birikintilerinden kaçının. Sık sık yapraklarını budayarak daha güçlü bir büyüme elde edebilirsiniz.")
  if (class_name[2:].strip()=="Kekik"):
    print ("Kekik, kurak ve sıcak bölgelerde doğal olarak yetişir, bu yüzden iyi drene edilen, hafif ve kumlu toprakları tercih eder. Güneşli bir alan seçin ve çok fazla sulamaktan kaçının. Tohumları ekmeden önce toprağı iyice hazırlayın ve ince bir toprak tabakasıyla örtün. Düzenli olarak sulayın, ancak sulama aralıklarını uzun tutarak toprağın kurumasını sağlayın.")
  if (class_name[2:].strip()=="Melisa"):
    print ("Melisa bitkisi gölgeye dayanıklı olup, yarı gölge ya da güneşli alanlarda yetişebilir. Hafif nemli, besin bakımından zengin ve iyi drene edilen toprakları tercih eder. Tohumları ilkbaharda ekebilirsiniz. Bitki büyüdükçe yanlardan budayarak daha fazla yaprak elde edebilirsiniz. Toprağı kurudukça sulamaya özen gösterin.")
  if (class_name[2:].strip()=="Biberiye"):
    print ("Biberiye, güneşi çok sever ve nemli olmayan, iyi drene edilmiş toprakta yetişir. Tohum yerine çeliklerle çoğaltılması daha yaygındır çünkü tohumdan çimlenmesi zaman alır. Çelikleri bahar aylarında toprağa dikerek biberiye yetiştirebilirsiniz. Bitkiyi fazla sulamaktan kaçının; toprağın kurumasını beklemek en iyisidir.")
  if (class_name[2:].strip()=="Lavanta"):
    print ("Lavanta, bol güneş ve kurak ortamları tercih eder. Kumlu ve kireçli toprakları sever. Tohumları ilkbaharda veya sonbaharda ekebilirsiniz. Tohumları çimlendirmek zor olabilir, bu yüzden lavanta yetiştirmek için en iyi yöntem, çelik dikimidir. Sulama yaparken toprağı fazla ıslatmamaya dikkat edin, kök çürümesine karşı hassastır.")
  if (class_name[2:].strip()=="Nane"):
    print ("Nane, yarı gölge ve nemli toprakları tercih eder. Tohum ya da köklerinden çoğaltabilirsiniz. Toprağı sürekli nemli tutun, ancak su birikintisi oluşmamasına dikkat edin. Nane bitkisi hızlı büyür ve genişler, bu yüzden saksıda veya sınırlandırılmış bir alanda yetiştirmek en iyisidir. Yaprakları toplamak için düzenli olarak budama yapabilirsiniz.")
  if (class_name[2:].strip()=="Papatya"):
    print ("Papatya, güneşli alanlarda ve hafif, iyi drene edilen topraklarda en iyi şekilde yetişir. İlkbaharda doğrudan toprağa tohum ekimi yapabilirsiniz. Hafif nemli toprakları sever, bu yüzden düzenli ancak hafif sulama yapın. Bitkiler çiçek açmaya başladığında, çiçekleri düzenli olarak toplayabilirsiniz.")
  if (class_name[2:].strip()=="Zencefil"):
    print ("Zencefil, nemli ve sıcak ortamlarda yetişir. Organik madde açısından zengin ve nemli toprakları sever. Köklerden çoğaltabilirsiniz, bunun için taze zencefil kökünü nemli toprağa gömüp bekleyebilirsiniz. Gelişim sürecinde toprağı sürekli nemli tutmaya dikkat edin, doğrudan güneş ışığından kaçının.")
  if (class_name[2:].strip()=="Ekinezya"):
    print (" Ekinezya, bol güneşli yerleri sever ve kuraklığa dayanıklıdır. Kumlu ve iyi drene edilen topraklarda en iyi şekilde büyür. İlkbaharda tohumları doğrudan dış mekana ekebilirsiniz. Sulama yaparken toprağın kurumasını bekleyin. Çiçeklenme döneminde çiçeklerini toplayarak çayı için kullanabilirsiniz.")
  return 1

  bitkibulucu("bilinmeyen.jpg")
