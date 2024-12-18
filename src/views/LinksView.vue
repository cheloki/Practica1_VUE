<template>
    <div class="about">
        <h5 class="mb-5 mt-5">Mis links</h5>
       
        <div>
        
            <div class="mb-3">
                <div class="input-group">
                    <span class="input-group-text" id="type-filter"><i
                            class="bi bi-filter "></i>  Filtrar por Nombre: </span>
                    <select class="form-select" @change="toFilter = $event.target.value">
                        <option value="">todos</option>
                        <option v-for="contact in contacts" :value="contact.name" :key="contact.name"> {{ contact.name }}</option>
                    </select>
                </div>
            </div>

            <div class="mb-3">
                <div class="input-group">
                    <span class="input-group-text" id="email-search-text"><i class="bi bi-search"></i> Buscar en Email </span>
                    <input type="search" class="form-control" id="email-search"
                           @search="this.toSearch = $event.target.value">
                </div>
            </div>
        </div>

        <div class="card">
            <div class="card-body">
                <form @submit.prevent="save()">
                    <table class="table table-striped">
                        <thead>
                        <tr>
                            <th >
                                <input type="text" class="form-control" id="id" v-model="newLink.id">
                            </th>
                            <th >
                                <input type="text" class="form-control" id="name" v-model="newLink.name">
                            </th>
                            <th>
                                <input type="email" class="form-control" id="email" v-model="newLink.email">
                            </th>
                            <th>
                                <input type="text" class="form-control" id="address" v-model="newLink.address">
                            </th>
                            <th>
                                <input type="text" class="form-control" id="phone" v-model="newLink.phone">
                            </th>
                            <th>
                                <input type="text" class="form-control" id="country" v-model="newLink.country">
                            </th>
                            <th>
                                <input type="text" class="form-control" id="city" v-model="newLink.city">
                            </th>
                            <th>
                                <button type="submit" class="btn btn-primary">Agregar</button>
                            </th>
                        </tr>
                        <tr>
                            <th scope="col">id</th>
                            <th scope="col">name</th>
                            <th scope="col">email</th>
                            <th scope="col">address</th>
                            <th scope="col">phone</th>
                            <th scope="col">country</th>
                            <th scope="col">city</th>
                            <th></th>
                        </tr>
                        </thead>

                        <tbody>
                        <tr v-for="(link,index) in getList()" :key="index">
                          <!-- <th scope="row">{{1+index}}</th>-->
                            <td>{{link.id}}</td>
                            <td>{{link.name}}</td>
                            <td><a :href="link.email">{{link.email}}</a></td>
                            <td>{{link.address}}</td>
                            <td>{{link.phone}}</td>
                            <td>{{link.country}}</td>
                            <td>{{link.city}}</td>
                            <td>
                                <button type="button" class="btn btn-info btn-sm" @click="edit(index+1)"><i
                                        class="bi bi-pen"></i></button>

                                <button type="button" class="btn btn-danger btn-sm" @click="remove(index)"><i
                                        class="bi bi-trash3"></i></button>
                            </td>
                        </tr>
                        </tbody>
                    </table>
                </form>
            </div>
        </div>

        <!--Modal-->  
        <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="editModalLabel">Editar</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <form @submit.prevent="saveEdit()">
                        <div class="modal-body" v-if="itemSelected">
                            <div class="mb-3">
                                <label for="updateId" class="form-label">Id:</label>
                                <input type="text" v-model="itemSelected.id" class="form-control"
                                id="updateId">
                            </div>
                            <div class="mb-3">
                                <label for="updateName" class="form-label">Name:</label>
                                <input type="text" v-model="itemSelected.name" class="form-control"
                                id="updateNombre">
                            </div>
                            <div class="mb-3">
                                <label for="updateEmail" class="form-label">Email:</label>
                                <input type="text" v-model="itemSelected.email" class="form-control" id="updateEmail">
                            </div>
                            <div class="mb-3">
                                <label for="updateDireccion" class="form-label">Address:</label>
                                <input type="text" v-model="itemSelected.address" class="form-control"
                                       id="updateDireccion">
                            </div>
                            <div class="mb-3">
                                <label for="updateTelefono" class="form-label">Phone:</label>
                                <input type="text" v-model="itemSelected.phone" class="form-control"
                                       id="updateTelefono">
                            </div>
                            <div class="mb-3">
                                <label for="updatePais" class="form-label">Country:</label>
                                <input type="text" v-model="itemSelected.country" class="form-control"
                                       id="updatePais">
                            </div>
                            <div class="mb-3">
                                <label for="updateCiudad" class="form-label">City:</label>
                                <input type="text" v-model="itemSelected.city" class="form-control"
                                       id="updateCiudad">
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                            <button type="submit" class="btn btn-primary">Guardar cambios</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                types: ['frontend', 'backend', 'system', 'library'],
                newLink: {
                    id: '',
                    name: '',
                    email: '',
                    address: '',
                    phone: '',
                    country: '',
                    city: ''

                },
                contacts: [
                {
                id: 1, 
                name: "Alice Johnson", 
                email: "alice.johnson@example.com", 
                address: "123 Maple Street", 
                phone: "123-456-7890", 
                country: "USA", 
                city: "New York"
                }, 
                {
                id: 2, 
                name: "Bob Smith", 
                email: "bob.smith@example.com", 
                address: "456 Oak Avenue", 
                phone: "987-654-3210", 
                country: "Canada", 
                city: "Toronto"
                }, 
                {
                id: 3, 
                name: "Carol White", 
                email: "carol.white@example.com",
                address: "789 Pine Road", 
                phone: "555-123-4567", 
                country: "UK", 
                city: "London"
                }, 
                {
                id: 4, 
                name: "David Brown", 
                email: "david.brown@example.com", 
                address: "321 Elm Street", 
                phone: "444-555-6666", 
                country: "Australia", 
                city: "Sydney"
                }, 
                {
                id: 5, 
                name: "Emily Davis", 
                email: "emily.davis@example.com", 
                address: "654 Spruce Lane", 
                phone: "333-444-5555", 
                country: "USA", 
                city: "Los Angeles"
                }

                ],
                itemSelected: null,
                indexSelected: null,
                typeFilter: '',
                toFilter: '',
                toSearch: ''
            }
        },
        methods: {
            
            save() {
                // Validar que los campos no estén vacíos
                if (this.newLink.id && this.newLink.name && this.newLink.email && this.newLink.address && this.newLink.phone && this.newLink.country && this.newLink.city) {
                    // Agregar un nuevo objeto al array
                    /**
                     * const newObj {type: this.newlink.type}
                     * 
                     * const newObj = {...this.newLink}
                     * */
                    this.contacts.push({...this.newLink});
                    // Limpiar los campos del formulario
                    this.newLink.id = '';
                    this.newLink.name = '';
                    this.newLink.email = '';
                    this.newLink.address ='';
                    this.newLink.phone ='';
                    this.newLink.country ='';
                    this.newLink.city ='';
                } else {
                    alert("Por favor, completa todos los campos.");
                }
            },
            remove(index) {
                if (confirm("¿Está seguro de eliminar este contacto?")) {
                    this.contacts.splice(index, 1);
                }
            },
            edit(index) {
                this.itemSelected = {...this.contacts[index-1]};
                this.indexSelected = index;
                const editModal = new bootstrap.Modal(document.getElementById('editModal'));
                editModal.show();
            },
            saveEdit() {
                this.contacts[this.indexSelected] = {...this.itemSelected};

                const modalElement = document.getElementById('editModal');
                const modalInstance = bootstrap.Modal.getInstance(modalElement) || new bootstrap.Modal(modalElement);
                modalInstance.hide();

                this.itemSelected = null;
                this.indexSelected = null;
            },
       
            getList() {
            
                let result = this.contacts.filter((item) => {
                    if (this.toSearch) {
                        return item.email.includes(this.toSearch);
                       // return item.description.includes(this.toSearch);
                    }
                    return true;
                });

                return result.filter((item) => {
                    if (this.toFilter) {
                        return item.name === this.toFilter;
                    }
                    return true;
                });
                
                // return this.linksArray;
                
            }

        }
    }
</script>

<style>
    .btn {
        margin-right: 3px;
    }
    .card{
        overflow-x: auto;
    }
</style>