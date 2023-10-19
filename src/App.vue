<template>
  <div id="app">
   
    <div id="container">
    <h1>Cifrado y Descifrado Escítala</h1>
      <label for="plaintext">Texto a cifrar/descifrar:</label>
      <textarea v-model="plaintext" rows="4" cols="50"></textarea>

      <label for="key">Clave (número de vueltas):</label>
      <input type="number" v-model="key">
      
      <button @click="encrypt">Cifrar</button>
      <button @click="decrypt">Descifrar</button>

      <h2>Resultado:</h2>
      <div id="result">{{ result }}</div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      plaintext: "",
      key: 4,
      result: "",
    };
  },
  methods: {
    encrypt() {
      const encryptedText = this.escitalaEncrypt(this.plaintext, this.key);
      this.result = "Texto cifrado: " + encryptedText;
    },
    decrypt() {
      const decryptedText = this.escitalaDecrypt(this.plaintext, this.key);
      this.result = "Texto descifrado: " + decryptedText;
    },
    escitalaEncrypt(text, key) {
      let result = "";
      for (let i = 0; i < key; i++) {
        for (let j = i; j < text.length; j += key) {
          result += text[j];
        }
      }
      return result;
    },
    escitalaDecrypt(text, key) {
      const columns = Math.ceil(text.length / key);
      const rows = key;
      const missingChars = columns * rows - text.length;

      let result = new Array(rows);
      for (let i = 0; i < rows; i++) {
        result[i] = new Array(columns);
      }

      let row = 0;
      let col = 0;
      for (let i = 0; i < text.length; i++) {
        result[row][col] = text[i];
        col++;
        if ((col === columns) || (col === columns - 1 && row >= rows - missingChars)) {
          col = 0;
          row++;
        }
      }

      let decryptedText = "";
      for (let i = 0; i < columns; i++) {
        for (let j = 0; j < rows; j++) {
          if (result[j][i] !== undefined) {
            decryptedText += result[j][i];
          }
        }
      }

      return decryptedText;
    },
  },
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

#container {
  text-align: center;
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(1, 4, 0, 0.4);
  width: 80%;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.form {
  margin-top: 20px;
}

h1 {
  color: #333;
  margin-top: 40px; /* Añade margen superior para centrar el título */
  text-align: center; /* Centra horizontalmente el título */
}

label {
  display: block;
  margin-top: 10px;
  color: #555;
}

textarea, input {
  width: 50%;
  padding: 10px;
  margin-top: 5px;
  border: 5px solid #ff0080;
  border-radius: 5px;
}

button {
  padding: 10px 30px;
  background-color: #ff0080;
  color: #fff;
  border: none;
  cursor: pointer;
  margin-top: 20px;
  border-radius: 5px;
}

button:hover {
  background-color: #ff0080;
}

#result {
  width: 50%;
  margin-top: 20px;
  padding: 10px;
  background-color: #eee;
  color: #ff0080;
  border: 1px solid #ff0080;
  border-radius: 5px;
}
</style>