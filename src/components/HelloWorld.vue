<template>
  <div class="container">
    <div class="card">
      <div class="left-side">
        <h1 class="lead">My <span style="letter-spacing: 8px;">QRCode</span></h1>
        <div class="form-group">
          <label>Background Color</label>
          <input type="color" v-model="background" class="form-control input-color" />
        </div>
        <div class="form-group">
          <label>Foreground Color</label>
          <input type="color" v-model="foreground" class="form-control input-color" />
        </div>
        <div class="form-group two-col">
          <div class="left-form">
            <label>ECC Level</label>
            <select v-model="level" class="form-control">
              <option value="L" selected>L</option>
              <option value="M">M</option>
              <option value="Q">Q</option>
              <option value="H">H</option>
            </select>
          </div>
          <div class="right-form">
            <label>Size(px)</label>
            <input v-model="size" type="number" class="form-control">
          </div>
        </div>
        <div class="form-group">
          <label>Value</label>
          <textarea v-model="value" class="form-control" rows="5"></textarea>
        </div>
        <div class="form-group">
          <button @click="saveQRCode" class="btn btn-desktop">Save QRCode</button>
        </div>
      </div>
      <div class="right-side">
        <qrious
          v-bind:background="background"
          v-bind:foreground="foreground"
          v-bind:level="level"
          v-bind:size="size"
          v-bind:value="value"
        />
        <button @click="saveQRCode" class="btn btn-mobile">Save QRCode</button>
      </div>
    </div>
  </div>
</template>

<script>
import qrious from 'vue-qrious';

export default {
  name: 'HelloWorld',
  components: {
    qrious
  },
  methods: {
    saveQRCode(){
      var QRCode = document.querySelector('.right-side img').src;
      var blob = this.dataURLToBlob(QRCode);
      var url = window.URL.createObjectURL(blob);

      var a = document.createElement("a");
      a.style = "display: none";
      a.href = url;
      a.download = "qrcode.png";

      document.body.appendChild(a);
      a.click();

      window.URL.revokeObjectURL(url);
    },
    dataURLToBlob(dataURL){
      // Code taken from https://github.com/ebidel/filer.js
      var parts = dataURL.split(';base64,');
      var contentType = parts[0].split(":")[1];
      var raw = window.atob(parts[1]);
      var rawLength = raw.length;
      var uInt8Array = new Uint8Array(rawLength);

      for (var i = 0; i < rawLength; ++i) {
        uInt8Array[i] = raw.charCodeAt(i);
      }

      return new Blob([uInt8Array], { type: contentType });
    }
  },
  data() {
    return {
      background: '#ffffff',
      foreground: '#000000',
      level: 'L',
      size: '200',
      value: 'https://bisloka.com'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .container{
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 40px 0;
  }

  .card{
    background-color: #fff;
    border-radius: .25rem;
    padding: 28px;
    box-shadow: 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -1px rgba(0,0,0,.06);
    width: 70%;
    display: flex;
  }

  .btn{
    appearance: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    border: 0;
    background-color: #4299e1;
    color: #fff;
    border-radius: .25rem;
    font-size: 18px;
    padding: 8px 25px;
    cursor: pointer;
    transition: all 0.3s;
  }

  .btn-mobile{
    display: none;
  }

  .btn-desktop{
    display: block;
  }

  .lead{
    font-weight: 300;
  }

  .left-side,
  .right-side{
    width: 50%;
  }

  .form-group{
    margin-bottom: 20px;
  }

  .form-group label{
    display: block;
    margin-bottom: 10px;
  }

  .form-control{
    background-color: #fff;
    border: 1px solid #e2e8f0;
    color: #4a5568;
    width: 100%;
    box-shadow: 0 1px 3px 0 rgba(0,0,0,.1), 0 1px 2px 0 rgba(0,0,0,.06);
    padding: 10px 8px;
  }

  .form-control.input-color{
    height: 100px;
    padding: 5px;
    cursor: pointer;
  }

  .form-control.two-col{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .left-form,
  .right-form{
    width: 50%;
    display: inline-block;
  }

  .left-form{
    padding-right: 15px;
  }

  .right-side{
    display: flex;
    flex-direction: column;
  }

  .right-side img{
    margin: auto;
    max-width: 100%;
  }

  @media screen and (max-width: 780px){
    .btn-desktop{
      display: none;
    }

    .btn-mobile{
      display: block;
      margin-top: 15px;
    }

    .card{
      flex-direction: column;
      width: 100%;
    }

    .left-side,
    .right-side{
      width: 100%;
    }

    .right-side{
      display: block;
    }

    .container{
      padding: 0 16px;
    }
  }
</style>
