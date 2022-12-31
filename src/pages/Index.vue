<template>
  <q-page class="flex flex-center upload-page">
    <div calss="header-div">
      <form id="fileForm"><input type="file" name id="fileInput" />
      </form>
    </div>
    <div class="main-content">
      <div class="main-content__left">
        <canvas id="makerCanvas"></canvas>
      </div>
      <div class="main-content__right">
        <q-btn color="primary" label="영어" @click="function () { clickToTranslateButton('en') }" />
        <q-btn color="primary" label="한국어" @click="function () { clickToTranslateButton('ko') }" />

        <div class="original-text">
          <span>번역할 문장 : </span>
          <span v-html="originalText"></span>
        </div>
        <div class="translate-text">
          <span>번역 된 문장 : </span>
          <span v-html="translateText"></span>
          <!-- <q-input outlined v-model="translateText" label="translate text" /> -->
        </div>
        <!-- <div class="q-mt-lg">
          <q-btn color="primary" label="텍스트입력하기" @click="addToText" />
        </div> -->
        <div class="q-mt-lg">
          <q-btn color="primary" label="저장하기" @click="saveToImage" />
        </div>
      </div>
    </div>
  </q-page>
</template>
<script>
import ComputedMixin from "../ComputedMixin";
import UtilMethodMixin from "../UtilMethodMixin";
export default {
  mixins: [ComputedMixin, UtilMethodMixin],
  data () {
    return {
      imageUrl: "",
      originalText: [],
      translateText: []
    }
  },
  mounted () {


    window.makerCanvas = new fabric.Canvas("makerCanvas", {
      name: "makerCanvas",
      opcity: 1,
      preserveObjectStacking: true,
      selection: false,
      isDrawingMode: false,
      skipOffscreen: false,
    });

    window.makerCanvas.setDimensions({
      width: document.querySelector(".main-content__left").clientWidth,
      height: document.querySelector(".main-content__left").clientHeight
    });
    window.makerCanvas.requestRenderAll();
    this.imageInit()

  },
  methods: {
    saveToImage () {
      const thisObj = this;
      let cropped = document.createElement("img");
      let chatArray = []
      cropped.onload = () => {
        var a = document.createElement("a"); //Create <a>
        a.href = cropped.src; //Image Base64 Goes here
        a.download = "Image.png"; //File name Here
        a.click(); //Downloaded file
      }

      cropped.src = window.makerCanvas.toDataURL({
        top: 0,
        left: 0,
        width: window.makerCanvas.width,
        height: window.makerCanvas.height,
        format: 'png',
        multiplier: 1,
        enableRetinaScaling: true
      });
    },
    addToText (text, top, left) {
      // let newString = ""
      // for (let index = 0; index < this.translateText.length; index++) {
      //   const element = this.translateText[index];
      //   newString = newString + element
      //   if (index != this.translateText.length - 1) {
      //     console.log(element)
      //     newString = newString + "\n"
      //   }

      // }
      const textObj = new fabric.IText(text, {
        fontSize: 28,
        backgroundColor: "#ffffff",
        borderColor: "#000000",
        active: true,
        top,
        left,
        type: "text-input",
        scaleX: 1,
        scaleY: 1,
        fontFamily: "Lexend",
        lockScalingFlip: true,
        originX: "center",
        originY: "center",
      });
      window.makerCanvas.add(textObj)
      window.makerCanvas.requestRenderAll()

    },
    clickToTranslateButton (lagnCode) {
      const thisObj = this;
      if (lagnCode == "en") {

        let objects = window.makerCanvas.getObjects().filter(i => !i.image);
        Array.from(objects).forEach(function (currentObject) {
          window.makerCanvas.remove(currentObject);
        });
        window.makerCanvas.requestRenderAll();
        return false;
      }
      let cropped = document.createElement("img");
      let chatArray = []
      cropped.onload = () => {
        console.log(cropped.src)

        var img = new Image();
        img.src = cropped.src;
        img.onload = function () {
          OCRAD(img, function (text) {
            console.log(text)
            chatArray.push(text.split("\n"))
            chatArray = chatArray[0].filter(i => i !== "" && i !== "F _ __ _" && i !== "/. `" && i !==
              "/" && i !==
              "F _ _" && i !==
              "._/" && i !==
              "." && i !==
              "/__" && i !==
              "|' . '" && i !==
              "F" && i !==
              ". /" && i !==
              "/_'" && i !==
              "/." && i !==
              "/ , '_" && i !==
              ",i ," && i !==
              ",\" , \\" && i !==
              ", 7HEY DA7ING" && i !==
              ", 7HEY DA7ING_" && i !==
              "_ , 7HEY DA7ING _" && i !==
              "lrf_< . .'" && i !==
              "rf /" && i !==
              "|").map(i => {
                let newString = i.replace("| | ", "").replace("_ ", "")
                  .replace(" _", "")
                  .replace("? ", "")
                  .replace("'| ", "")
                  .replace("'| ", "")
                return newString

              })
            console.log(chatArray)
            thisObj.originalText = chatArray
            let newArray = [
              chatArray[0] + chatArray[1],
              chatArray[2] + " " + chatArray[3],
              chatArray[4] + chatArray[5],
              chatArray[6] + chatArray[7] + chatArray[8]
            ]
            thisObj.originalText = newArray

            newArray.map(i => {

              thisObj.translateAPI(i, lagnCode, function (result) {
                let text = result.ko
                console.log(text)
                let top = 0
                let left = 0

                if (text == "그들은...데이트?") {
                  left = 158;
                  top = 164
                  var square = new fabric.Rect({
                    scaleX: 1.4356435643564356,
                    scaleY: 0.8811881188118812,
                    width: 100,
                    height: 100,
                    left: 78,
                    top: 118,
                    fill: '#ffffff'

                  });
                  window.makerCanvas.add(square)
                  window.makerCanvas.moveTo(square, 1);
                }
                if (text == "왜 그들은 사귀지 않는거죠?") {
                  text = "왜 그들은 사귀지" + "\n" + "않는거죠?"
                  left = 253;
                  top = 557
                  var square = new fabric.Rect({
                    scaleX: 1.4356435643564356,
                    scaleY: 0.8811881188118812,
                    width: 100,
                    height: 100,
                    left: 331,
                    top: 196,
                    fill: '#ffffff'

                  });
                  window.makerCanvas.add(square)
                  window.makerCanvas.moveTo(square, 1);
                }
                if (text == "왜 그들은 데이트를 안하지?") {
                  text = "왜 그들은 데이트를" + "\n" + "하지 않는거죠??"
                  left = 253;
                  top = 557
                  var square = new fabric.Rect({
                    scaleX: 1.4356435643564356,
                    scaleY: 0.8811881188118812,
                    width: 100,
                    height: 100,
                    left: 331,
                    top: 196,
                    fill: '#ffffff'

                  });
                  window.makerCanvas.add(square)
                  window.makerCanvas.moveTo(square, 1);
                }
                if (text == "왜 그렇지 않은가.") {
                  text = "왜 그들은 데이트를" + "\n" + "하지 않는거죠??"
                  left = 253;
                  top = 557
                  var square = new fabric.Rect({
                    scaleX: 1.4356435643564356,
                    scaleY: 0.8811881188118812,
                    width: 100,
                    height: 100,
                    left: 331,
                    top: 196,
                    fill: '#ffffff'

                  });
                  window.makerCanvas.add(square)
                  window.makerCanvas.moveTo(square, 1);
                }
                if (text == "나도 그걸 알고 싶어") {
                  text = "나도 그걸" + "\n" + "알고 싶어"

                  top = 704;
                  left = 415
                  var square = new fabric.Rect({
                    scaleX: 2.5694238733635486,
                    scaleY: 0.32673267326732675,
                    width: 100,
                    height: 100,
                    left: 288,
                    top: 689,
                    fill: '#ffffff'

                  });
                  window.makerCanvas.add(square)
                  window.makerCanvas.moveTo(square, 1);
                }
                if (text == "난 그렇게 생각 안 해.") {
                  text = "난 그렇게" + "\n" + "생각 안 해."
                  left = 403;
                  top = 240
                  var square = new fabric.Rect({
                    scaleX: 2.00990099009901,
                    scaleY: 1.0657104171955656,
                    width: 100,
                    height: 100,
                    left: 313,
                    top: 644,
                    fill: '#ffffff'

                  });
                  window.makerCanvas.add(square)
                  window.makerCanvas.moveTo(square, 1);
                }
                if (text == "그렇게 생각하지 않아요") {
                  text = "난 그렇게" + "\n" + "생각 안 해."
                  left = 403;
                  top = 240
                  var square = new fabric.Rect({
                    scaleX: 2.00990099009901,
                    scaleY: 1.0657104171955656,
                    width: 100,
                    height: 100,
                    left: 313,
                    top: 644,
                    fill: '#ffffff'

                  });
                  window.makerCanvas.add(square)
                  window.makerCanvas.moveTo(square, 1);
                }
                // if(i)
                thisObj.translateText.push(text)
                thisObj.addToText(text, top, left)
              })

            })
          })
        }
      }

      cropped.src = window.makerCanvas.toDataURL({
        top: 0,
        left: 0,
        width: window.makerCanvas.width,
        height: window.makerCanvas.height,
        format: 'png',
        multiplier: 1,
        enableRetinaScaling: true
      });
    },
    clickToTranslateButton4 () {
      this.originalText = "I WANT TO KNOW,"

      const thisObj = this;
      this.translateAPI(this.originalText, function (result) {
        thisObj.translateText = result.ko
        console.log(result)
        thisObj.addToText()
      })
    },
    clickToTranslateButton5 () {
      this.originalText = "TOO"

      const thisObj = this;
      this.translateAPI(this.originalText, function (result) {
        thisObj.translateText = result.ko
        console.log(result)
        thisObj.addToText()
      })
    },
    translateAPI (text, resultLang, cb) {
      var apiUrl = "http://localhost:3000";
      $.ajax({
        type: "GET",
        beforeSend: function (request) {
          request.setRequestHeader("content-type", "text/javascript");
        },
        url: apiUrl + "/translate",
        data: { text, sendLang: "en", resultLang },
        dataType: "json",
        contentType: "application/json; charset=utf-8",
        success: function (data) {
          var data = JSON.parse(data);
          if (cb) {
            cb(data);
          }
        }
      });
    },
    rotateImage () {
      let currentCanvas = window.clipCanvas ? window.clipCanvas : window.makerCanvas
      let rotatedImg = currentCanvas.getObjects().filter(item => item.filterImage)[0];
      if (!window.rotateImageScaleX) {
        window.rotateImageScaleX = rotatedImg.scaleX;
        window.rotateImageScaleY = rotatedImg.scaleY;
      }
      let scaleValue = 1;
      let left = currentCanvas.width / 2;
      let top = currentCanvas.height / 2;
      window.rotateValue += 90;
      if (!rotatedImg) {
        return false;
      }
      if (window.rotateValue % 180 === 90) {
        if (
          rotatedImg.width <
          rotatedImg.height
        ) {
          scaleValue = (currentCanvas.width < rotatedImg.height
            ? currentCanvas.width
            : rotatedImg.width * window.rotateImageScaleX) / rotatedImg.height

        } else {

          scaleValue =
            (currentCanvas.height > rotatedImg.width
              ? currentCanvas.height
              : rotatedImg.height * window.rotateImageScaleX) / rotatedImg.width
        }
      } else {
        if (
          rotatedImg.width <
          rotatedImg.height
        ) {
          scaleValue = (currentCanvas.height < rotatedImg.height
            ? currentCanvas.height
            : rotatedImg.height * window.rotateImageScaleY) / rotatedImg.height
        } else {
          scaleValue =
            (currentCanvas.width < rotatedImg.height
              ? currentCanvas.width
              : rotatedImg.width * window.rotateImageScaleX) / rotatedImg.width
        }
      }

      if (currentCanvas.width < rotatedImg.width * scaleValue) {
        scaleValue =
          currentCanvas.width
          / rotatedImg.width
      } else if (currentCanvas.height < rotatedImg.height * scaleValue) {
        scaleValue =
          currentCanvas.height
          / rotatedImg.height
      }
      if (window.rotateValue == 360) {
        window.rotateValue = 0;
      }

      rotatedImg._setOriginToCenter();
      rotatedImg.setCoords();
      if (scaleValue > 1) {
        scaleValue = 1;
      }

      console.log(scaleValue)
      rotatedImg.set({
        left,
        top,
        angle: window.rotateValue,
        scaleX: scaleValue,
        scaleY: scaleValue,
        rotateAngle: window.rotateValue,
        originX: "center",
        originY: "center",
        filterImage: true
      });
      currentCanvas.requestRenderAll();
    },
    imageLoaded ({ url }) {
      const thisObj = this;
      return new Promise((resolve, reject) => {
        thisObj.imageUrl = url
        fabric.util.loadImage(
          url,
          async function (oImgSrc) {
            let imageDOM = document.createElement("img");
            let img = document.createElement("img");
            img.onload = function () {
              // Dynamically create a canvas element
              let canvas = document.createElement("canvas");
              const imgMaxSize = Math.max(img.width, img.height);
              let reszieImageWidth = img.width;
              let reszieImageHeight = img.height;

              window.originalImageSize = { width: img.width, height: img.height };

              let imageMaxSize = fabric.maxTextureSize;

              if (imgMaxSize >= imageMaxSize) {
                const resizeValue = imageMaxSize / imgMaxSize;
                reszieImageWidth = img.width * resizeValue;
                reszieImageHeight = img.height * resizeValue
              }
              canvas.width = reszieImageWidth;
              canvas.height = reszieImageHeight;

              let ctx = canvas.getContext("2d");
              ctx.drawImage(img, 0, 0, reszieImageWidth, reszieImageHeight);
              let dataurl = canvas.toDataURL({
                width: reszieImageWidth,
                height: reszieImageHeight
              });
              imageDOM.src = dataurl;
            }
            img.src = oImgSrc.src;
            imageDOM.crossOrigin = "anonymous";
            imageDOM.onload = () => {
              drawPhotoImage(imageDOM)
            }
            async function drawPhotoImage (imageData) {
              let oImg = new fabric.Image(imageData);
              oImg.hoverCursor = "default"
              oImg.selectable = false;
              oImg.image = true;
              window.object = oImg;
              window.object.clipImage = true;
              window.object.filterImage = true;
              window.rotateValue = 270;

              console.log("add image!")
              if (window.clipCanvas) {
                await window.clipCanvas.add(oImg);
                console.log("requestRender!")
                await window.clipCanvas.requestRenderAll();
              } else if (window.makerCanvas) {
                await window.makerCanvas.add(oImg);
                console.log("requestRender!")
                await window.makerCanvas.requestRenderAll();
              }
              thisObj.rotateImage();
              resolve();
            }

          },
          { crossOrigin: "anonymous" }
        );
        //画像読み込み関数

        window.rotateValue = 0;
      });
    },
    imageInit () {
      const thisObj = this;
      return new Promise((resolve) => {
        //画像の読み込み
        const fileFormDOM = document.querySelector('#fileForm');
        fileFormDOM.addEventListener('change', changedFile);
        function changedFile (e) {
          thisObj.clearCanvas();
          let tgt = e.target || window.event.srcElement,
            files = tgt.files;
          if (FileReader && files && files.length) {
            let fr = new FileReader();
            fr.onload = async function () {
              window.imageData = fr.result;
              window.originalImageData = fr.result;
              await thisObj.imageLoaded({ url: window.imageData }).then(resolve).catch(() => { });
            };
            fr.readAsDataURL(files[0]);
          }
        }
      });
    },
    clearCanvas () {
      let objects = window.makerCanvas.getObjects();
      Array.from(objects).forEach(function (currentObject) {
        window.makerCanvas.remove(currentObject);
      });
    }

  }
};
</script>

<style lang="scss">
.upload-page {
  display: flex;
  flex-direction: column;

  .header-div {
    display: flex;
    flex: 0;
    height: 70px;
    padding: 10px 0;

  }

  .main-content {
    display: flex;
    flex: 1;
    background: #ddd;
    width: 100%;

    &__left {
      background: white;
      flex: 1;
    }

    &__right {
      flex: 1;
      padding: 20px;

      .original-text {
        margin-top: 20px;
        ;
      }

      .translate-text {
        margin-top: 20px;
        ;
      }
    }

  }
}
</style>
