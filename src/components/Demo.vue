<template>
  <div>
    <div>
      <h3>Demo export canvas to .docx file</h3>
    </div>
    <div style="margin-top: 60px">
      <canvas class="canvas"></canvas>
      <canvas class="canvas"></canvas>
      <canvas class="canvas"></canvas>
      <canvas class="canvas"></canvas>
      <canvas class="canvas"></canvas>
      <canvas class="canvas"></canvas>
      <canvas class="canvas"></canvas>
      <canvas class="canvas"></canvas>
      <canvas class="canvas"></canvas>
    </div>
    <div style="margin-top: 20px">
      <button @click="generateDocxFile">Download docx!</button>
    </div>
  </div>
</template>

<script>
import { saveAs } from 'file-saver';
import {
  AlignmentType,
  Document,
  ImageRun,
  Packer,
  PageBreak,
  Paragraph
} from 'docx';

// https://stackoverflow.com/questions/51026420/filereader-readastext-async-issues/51026615

export default {
  name: 'Demo',
  data() {
    return {
      blobData: [],
    }
  },
  mounted() {
    this.drawCanvas()
  },
  methods: {
    drawCanvas() {
      // Initial canvas draw
      let canvas = document.getElementsByClassName('canvas');
      if (canvas.length > 0) {
        for (let i = 0; i < canvas.length; i++) {
          let ctx = canvas[i].getContext('2d');
          ctx.beginPath();
          ctx.moveTo(0, 0);
          ctx.lineTo(300, 150);
          ctx.stroke();
        }
      }
    },

    generateDocxFile() {
      let canvas = document.getElementsByClassName('canvas');
      if (canvas.length > 0) {
        const that = this
        console.log('11111111111111111111111111111')

        for (let i = 0; i < canvas.length; i++) {
          canvas[i].toBlob((blobContent) => {
            let reader = new FileReader();
            reader.readAsDataURL(blobContent);
            reader.onload = async function () {
              console.log(22222222222222222222222)
              // Since it contains the Data URI, we should remove the prefix and keep only Base64 string
              const b64 = await reader.result.replace(/^data:.+;base64,/, '');
              that.blobData.push(b64)
            };
          })
        }
      }
      console.log(33333333333333333333333333333333)


      setTimeout(() => {
        const doc = this.createDocx()
        //
        Packer.toBlob(doc).then(blob => {
          saveAs(blob, 'example.docx');
        });
      }, 2000)
    },

    createDocx() {
      const vm = this
      return new Document({
        sections: [
          {
            children: [
              vm.createContactInfo()
            ]
          }
        ]
      });
    },

    createContactInfo() {

      console.log('this.blobData', this.blobData)

      let childrenElm = []
      Array.prototype.forEach.call(this.blobData, item => {
        const imageRun = new ImageRun({
          data: Buffer.from(item, 'base64'),
          transformation: {
            width: 200,
            height: 200
          },
          floating: {
            horizontalPosition: {
              offset: 1014400, // relative: HorizontalPositionRelativeFrom.PAGE by default
            },
            verticalPosition: {
              offset: 1014400, // relative: VerticalPositionRelativeFrom.PAGE by default
            },
          },
        })
        childrenElm.push(imageRun)

        const breakPageElm = new PageBreak()
        childrenElm.push(breakPageElm)
      });

      return new Paragraph({
        alignment: AlignmentType.CENTER,
        children: childrenElm
      });
    }
  }
}
</script>

<style scoped>
.canvas {
  width: 200px;
  height: 100px;
  padding: 10px;
  margin: 10px;
  border: 1px solid #000000;
}
</style>
