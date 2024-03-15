<template>
  <!--
  Template est un élément par défaut de VueJS
  Pour y inclure notre code HTML
  -->
  <div>
    <p>Saisie d'un client</p>

    <form id="clientForm">

      <label for="nom">Nom :</label>
      <input type="text" id="nom" name="nom"><br><br>

      <label for="prenom">Prénom :</label>
      <input type="text" id="prenom" name="prenom"><br><br>

      <label for="societe">Société :</label>
      <input type="text" id="societe" name="societe"><br><br>

      <button>Enregistrer</button>
    </form>


    <table>
      <thead>
      <tr>
        <th id="th-1" class="">ID</th>
        <th class="entete">Nom</th>
        <th class="entete">Prénom</th>
        <th class="entete">Société</th>
        <th class="entete">Actions</th>
      </tr>
      </thead>
      <tbody>
      <!--
      On fait un forNext sur les clients
      Recupérée depuis le fetch (/api/clients)
      -->
      <!-- Le "v-*" est spécifique à vueJS et n'est pas exploité
      en javascript ou HTML pur
      -->
      <tr v-for="client in clients">
        <!-- On affiche l'id, le nom ... -->
        <td>{{ client.id  }}</td>
        <td>{{ client.nom }}</td>
        <td>{{ client.prenom }}</td>
        <td>{{ client.societe }}</td>
        <td>
          <!-- On a un exemple de condition avec
          par exemple le client.id 1 qu'on ne peut pas supprimer
          -->
          <span v-if="client.id != 1">Supprimer</span>
        </td>
      </tr>
      </tbody>
    </table>

  </div>
</template>

<script>
// Notation propre à une nouvelle façon de coder
// en Javascript, qui est réutilisée par VueJS
export default {
  name: "exemple",
  // On crée une variable "partagée" pour l'application VueJS
  data() {
    return {
      clients: []
    }
  },
  /** quand la page se charge on exécute cette fonction */
  mounted() {
    /** On va récupérer la liste des clients **/
    fetch("http://localhost/api/clients")
        .then(response => response.json())
        .then((response) => {
          // On affecte la liste des clients récupérés
          // sur l'API avec la clé "hydra:member"
          // qui est dépendante de API-platform
          // le but étant de modifier this.clients
          // qui mettra à jour le v-for du tableau
          this.clients = response['hydra:member']
      })

    // on récupère le formulaire avec l'identifiant clientForm
    // et on lui affecte l'événement "submit" (à la soumission du formulaire)
    // et il déclenchera la fonction saveData
    document.getElementById('clientForm').addEventListener('submit', this.saveData)
  },
  methods: {
    async saveData(e) {
      e.preventDefault()
      let formData = new FormData(document.getElementById('clientForm'));
      let data = {};
      formData.forEach(function(value, key) {
        data[key] = value;
      });

      // On structure les données pour que l'API puisse
      // Les exploiter
      data = JSON.stringify(data)

      /**
       * On sauvegarde sur l'API
       * avec la méthode POST et les données dans "body"
       * comme stipulé dans http://localhost/api/docs
       * **/
      await fetch("http://localhost/api/clients", {
        method: "POST",
        headers: {
          "Content-Type": "application/ld+json",
        },
        body: data
      })

      /* On vide les différents champs du formulaire**/
      formData.forEach(function(value, key) {
        // on récupère l'élément par son nom
        document.getElementsByName(key)[0].value = ""
      })

      // on récupère nos clients, après sauvegarde
      await fetch("http://localhost/api/clients")
          .then(response => response.json())
          .then((response) => {
            this.clients = response['hydra:member']
          })
    }
  }
}
</script>

<style scoped>
/** On peut cibler des éléments avec le nom de la balise
 */
table {
  width: 100%;
  background: red;
}

/** On peut cibler des éléments avec l'attribut "id"
 */

#th-1 {
  background: green;
}

/** On peut cibler des éléments avec l'attribut "class"

 */
.entete {
  background: yellow;
}

/** on aligne les champs de saisie
en spécifiant une largeur fixe aux libellés
 */
label {
  width: 100px;
  display: inline-block;
}
</style>