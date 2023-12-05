<template>
  <div>
    <h1>Liste des Livres</h1>
    <button class="add" @click="showForm()">ajouter</button>

    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Titre</th>
          <th>Auteur</th>
          <th>Prix</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="book in books" :key="book.id">
          <td>{{ book.id }}</td>
          <td>{{ book.title }}</td>
          <td>{{ book.author }}</td>
          <td>{{ book.price }}</td>
            <td>
                <button class="delete" @click="confirmDelete(book.id)">Supprimer</button>
                <button class="update" @click="confirmEdit(book)">Modifier</button>
            </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Swal from 'sweetalert2';

export default {
  data() {
    return {
      books: [
        { id: 1, title: "Livre 1", author: "Auteur 1", price: 10.99 },
        { id: 2, title: "Livre 2", author: "Auteur 2", price: 15.99 },
      ],
    };
  },
  methods: {
    confirmDelete(id) {
      Swal.fire({
        title: 'Êtes-vous sûr de vouloir supprimer ce livre ?',
        text: "Cette action est irréversible !",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Oui, supprimer',
        cancelButtonText: 'Annuler'
      }).then((result) => {
        if (result.isConfirmed) {
          // Appeler la méthode pour supprimer le livre
          this.deleteBook(id);
        }
      });
    },
    deleteBook(id) {
      const index = this.books.findIndex((book) => book.id === id);
      if (index !== -1) {
        this.books.splice(index, 1);
        Swal.fire('Supprimé !', 'Le livre a été supprimé.', 'success');
      }
    },
    confirmEdit(book) {
      Swal.fire({
        title: 'Modifier le livre',
        html: `
          <input id="swal-input1" class="swal2-input" value="${book.title}" placeholder="Titre">
          <input id="swal-input2" class="swal2-input" value="${book.author}" placeholder="Auteur">
          <input id="swal-input3" class="swal2-input" value="${book.price}" placeholder="Prix">
        `,
        icon: 'info',
        showCancelButton: true,
        confirmButtonText: 'Modifier',
        cancelButtonText: 'Annuler',
        showLoaderOnConfirm: true,
        preConfirm: () => {
          const updatedTitle = document.getElementById('swal-input1').value;
          const updatedAuthor = document.getElementById('swal-input2').value;
          const updatedPrice = document.getElementById('swal-input3').value;

          // Mettre à jour le livre ici
          const updatedBook = {
            id: book.id,
            title: updatedTitle,
            author: updatedAuthor,
            price: updatedPrice,
          };
          this.updateBook(updatedBook); // Assurez-vous que cette méthode est définie

          // Vous pouvez également afficher un message de confirmation ici
          return new Promise(() => {
            Swal.fire('Modifié !', 'Le livre a été modifié.', 'success');
          });
        }
      });
    },
    updateBook(updatedBook) {
      // Trouver le livre correspondant dans le tableau books
      const bookIndex = this.books.findIndex((book) => book.id === updatedBook.id);

      if (bookIndex !== -1) {
        // Mettre à jour les données du livre avec les nouvelles données
        this.books[bookIndex] = { ...updatedBook };

      }
    },
    showForm() {
      Swal.fire({
        title: 'Ajouter un livre',
        html: `
          <input id="swal-input1" class="swal2-input"  placeholder="ID">
          <input id="swal-input2" class="swal2-input"  placeholder="Titre">
          <input id="swal-input3" class="swal2-input"  placeholder="Auteur">
          <input id="swal-input4" class="swal2-input"  placeholder="Prix">
        `,
        showCancelButton: true,
        confirmButtonText: 'Ajouter',
        cancelButtonText: 'Annuler',
        showLoaderOnConfirm: true,
        preConfirm: () => {
          const id = document.getElementById('swal-input1').value;
          const title = document.getElementById('swal-input2').value;
          const author = document.getElementById('swal-input3').value;
          const price = document.getElementById('swal-input4').value;

          // Mettre à jour le livre ici
          const newBook = {
            id: id,
            title: title,
            author: author,
            price: price,
          };
          this.addBook(newBook); 
          return new Promise(() => {
            Swal.fire('Ajouter !', 'Le livre a été ajouté.', 'success');
          });
        }
      });
    },
    addBook(newBook) {
      this.books.push(newBook);
    },
  }
}
</script>

<style>
body {
    font-family: Arial, sans-serif;
}
h1{
    margin-top: 70px;
}

table {
    width: 80%;
    border-collapse: collapse;
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
    margin-top: 50px;
    margin-left : 100px;
    text-align: center;

}

th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: center;
}

th {
    background-color: #f2f2f2;
}

tr:hover {
    background-color: #f5f5f5;
}


button{
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 3px;
    cursor: pointer;
    opacity: 0.8;
}
.delete {
    background-color: #f44336;    
    margin-left: 20px;
}

.add {
    background-color: #4CAF50;
}
.update{
 background-color: #4c56af;
 margin-left: 20px;

}
button:hover{
    opacity: 1;
}



h2 {
    text-align: center;
}


</style>
