<template>
  <div class="bodyPage">
    <div class="typeAction">
      <p  class="Ok" style="color: white; font-weight: bold;">Type d'action :</p>
      <div class="typeActionBouton" @click="filterByType('Apprendre')">Apprendre</div>
      <div class="typeActionBouton" @click="filterByType('Participer')">Participer</div>
      <div class="typeActionBouton" @click="filterByType('Entreprendre')">Entreprendre</div>
      <div class="typeActionBouton" @click="filterByType('Faire rayonner')">Faire rayonner</div>
      <div class="typeActionBouton" @click="filterByType('Inspirer')">S'inspirer</div>
    </div>

    <div class="resteAFaire">
      <p>Nouvelles actions : </p>
      <div class="resteAFaireBouton" style="">{{ countActionsAFaire }}</div>

      <p>Actions effectuées :</p>
      <div class="resteAFaireBouton" style="">{{ countActionsFaites }}</div>

    </div>

    <div class="row">
      <div v-for="post in filteredPosts" :key="post.id" class=" col-md-4 col-sm-20 mb-5">
        <div class="card">
          <div class="card-body" style="background-color: #DDECCB ;">
            <h3 class="card-title" style="font-weight: bold;">{{ post.title }}</h3>
            <div style="display: flex; flex-direction: row;">
              <p><img src="../public/photos/temps(1).png" alt="" style="max-width: 20px; margin-right: 5px;">{{ post.temps
              }}</p>
              <div class="typeActionDescription">
                <p style=" font-weight: bold; margin-top: 5%;">{{ post.type }}</p>
              </div>
            </div>
            <p class="card-text">Lorem ipsum dolor sit amet consectetur adipisicing elit. Nulla harum dolorum inventore eos cupiditate atque impedit eligendi ratione veniam, esse aliquam nesciunt totam? Quidem quis saepe velit excepturi dolore ad.</p>
            <a href="#" class="btn btn-success action" @click="togglePostStatus(post)">
              {{ post.isDone ? 'Fait' : 'A faire' }}
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Cookies from 'js-cookie';
export default {
  data() {
    return {
      loading: false,
      posts: [],
      error: null,
      id: null,
      selectedType: "", // Nouvelle propriété pour stocker le type d'action sélectionné
    };
  },
  methods: {
    filterByType(type) {
      this.selectedType = type;
    },
    togglePostStatus(post) {
      post.isDone = !post.isDone;
      Cookies.set(`post_${post.id}`, post.isDone ? 'Fait' : 'A faire');
    },
  },
  computed: {
    filteredPosts() {
      if (!this.selectedType) {
        return this.posts; // Si aucun type n'est sélectionné, renvoyer tous les posts
      } else {
        return this.posts.filter((post) => post.type === this.selectedType);
      }
    },
    countActionsAFaire() {
      return this.posts.filter((post) => !post.isDone).length;
    },
    countActionsFaites() {
      return this.posts.filter((post) => post.isDone).length;
    },
  },
  async created() {
    try {
      this.loading = true;
      const response = await axios.get('https://my-json-server.typicode.com/bastien64/m/posts');
      this.loading = false;

      // Map response data and set the `isDone` property based on the cookies
      this.posts = response.data.map((post) => {
        const cookieValue = Cookies.get(`post_${post.id}`);
        if (cookieValue === 'Fait') {
          post.isDone = true;
        } else {
          post.isDone = false;
        }
        return post;
      });
    } catch (error) {
      this.loading = false;
      this.error = `Une erreur s'est produite : ${error.message}`;
    }

    const params = new URLSearchParams(window.location.search);
    this.id = params.get('id');
  },
};
</script>

<style>
@media only screen and (min-width: 767px) {

  .bodyPage {
  width: 100vw;
  margin-top: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-size: 2vh;
}

.typeAction {
  width: 90vw;
  height: 20vh;
  background-color: rgb(49, 104, 54);
  display: flex;
  justify-content: center;
  align-items: center;
  color: black;
  font-weight: bold;
  border-radius: 20px;
  box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.13);
}

.typeActionBouton {
  width: 150px;
  height: 40px;
  background-color: rgb(221, 236, 203);
  display: flex;
  justify-content: center;
  align-items: center;
  color: black;
  font-weight: bold;
  border-radius: 10px;
  margin-left: 5%;
  box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.13);
}

.typeActionBouton:hover {
  background-color: rgb(53, 119, 58);
  color: white;
}


.resteAFaire {
  margin-top: 2%;
  width: 90vw;
  height: 20vh;
  display: flex;
  flex-direction: row;

}

.resteAFaireBouton {
  width: 30px;
  height: 30px;
  background-color: rgb(221, 236, 203);
  display: flex;
  justify-content: center;
  align-items: center;
  color: black;
  font-weight: bold;
  border-radius: 10px;
  margin-left: 2%;
  margin-right: 2%;
  box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.13);
}

.typeActionDescription {
  width: 150px;
  height: 40px;
  background-color: rgb(49, 104, 54);
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-weight: bold;
  border-radius: 10px;
  margin-left: 5%;
  box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.13);
}
.row {
  max-width: 90vw;
}
}

@media only screen and (max-width: 767px){
  .bodyPage {
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.typeAction {
  width: 50vw;
  height: 50vh;
  background-color: rgb(49, 104, 54);
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  color: black;
  font-weight: bold;
}

.typeActionBouton {
  width: 40vw;
  height: 40px;
  background-color: rgb(221, 236, 203);
  display: flex;
  justify-content: center;
  align-items: center;
  color: black;
  font-weight: bold;
  margin-top: 5%;
  border-radius: 10px;
  margin-left: 5%;
}

.typeActionBouton:hover {
  background-color: rgb(49, 104, 54);
  color: white;
}


.resteAFaire {
  margin-top: 2%;
  width: 70vw;
  height: 20vh;
  display: flex;
  flex-direction: row;

}

.resteAFaireBouton {
  width: 50px;
  height: 30px;
  background-color: rgb(221, 236, 203);
  display: flex;
  justify-content: center;
  align-items: center;
  color: black;
  font-weight: bold;
  border-radius: 10px;
  margin-left: 2%;
  margin-right: 2%;
}

.typeActionDescription {
  width: 150px;
  height: 40px;
  background-color: rgb(49, 104, 54);
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-weight: bold;
  border-radius: 10px;
  margin-left: 5%;
}
}</style>