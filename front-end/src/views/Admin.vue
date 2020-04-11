<template>
<div class="admin">
  <h1>Monster Editor</h1>
    <div class="heading">
      <div class="circle">1</div>
      <h2>Add a Creature</h2>
    </div>
    <div class="add">
      <div class="upload" v-if="addItem">
        <h2>{{addItem.title}} CR: {{addItem.cr}}</h2>
        <img :src="addItem.path" />
        <p>- {{addItem.descript}}</p>
      </div>

      <div class="form">
        <input v-model="title" placeholder="Title">
        <input v-model="cr" placeholder="Challenge Rating">
        <input v-model="descript" placeholder="Descript">
        <p></p>
        <input type="file" name="photo" @change="fileChanged" class="rounded">
        <button @click="upload" class="rounded">Upload</button >
      </div>


    </div>
    <div class="heading">
      <div class="circle">2</div>
      <h2>Edit/Delete an Creature</h2>
    </div>
    <div class="edit">
      <div class="upload" v-if="findItem">
        <input v-model="findItem.title">
        <input v-model="findItem.cr" >
        <input v-model="findItem.descript" >
        <p></p>
        <img :src="findItem.path" />
      </div>

      <div class="form">
        <input class="rounded" v-model="findTitle" placeholder="Search">
        <div class="suggestions" v-if="suggestions.length > 0">
          <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}
          </div>
        </div>
        <div class="actions" v-if="findItem">
          <button @click="deleteItem(findItem)" class="rounded3">Delete</button>
          <button @click="editItem(findItem)" class="rounded2">Edit</button>
        </div>
      </div>


    </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Admin',

  data() {
      return {
        title: "",
        descript: "",
        cr:"",
        file: null,
        addItem: null,
        findTitle: "",
        findItem: null,
        items: [],
      }
    },

    computed: {
    suggestions() {
      let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.title > b.title);
    }
  },

  created() {
this.getItems();
},

    methods: {
      async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          cr: this.findItem.cr,
          descript: this.findItem.descript,
        });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },

      async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
      selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
    },
      async getItems() {
  try {
    let response = await axios.get("/api/items");
    this.items = response.data;
    return true;
  } catch (error) {
    console.log(error);
  }
},

      fileChanged(event) {
        this.file = event.target.files[0]
      },
      async upload() {
        try {
          const formData = new FormData();
          formData.append('photo', this.file, this.file.name)
          let r1 = await axios.post('/api/photos', formData);
          let r2 = await axios.post('/api/items', {
            title: this.title,
            cr: this.cr,
            descript: this.descript,
            path: r1.data.path
          });
          this.addItem = r2.data;

        } catch (error) {
          console.log(error);
        }
      },
    }
}




</script>

<style scoped>
/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}

.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}

.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

button {
  margin-top: 4px;

}

.rounded {
  border-radius: 8px;
}

.rounded2 {
  margin-left: 12px;
  border-radius: 8px;
}

.rounded3 {
  margin-top: 32px;
  border-radius: 8px;
}

input {
  margin-top: 4px;
  display: block;
}

.form {
  margin-right: 50px;

}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
  margin-right: 32px;
}
</style>
