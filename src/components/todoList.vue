<template>
  <p>Todo Application</p>
  <table style="border-collapse: collapse; width: 100%; margin: 20px 0; text-align: left; font-family: Arial, sans-serif;">
    <thead style="background-color: #f4f4f4; color: #333; font-weight: bold;">
    <tr>
      <th style="padding: 12px; border: 1px solid #ccc;">Text</th>
      <th style="padding: 12px; border: 1px solid #ccc;">Description</th>
      <th style="padding: 12px; border: 1px solid #ccc;">Completed</th>
      <th style="padding: 12px; border: 1px solid #ccc;">Delete</th>
      <th style="padding: 12px; border: 1px solid #ccc;">Edit</th>

    </tr>
    </thead>
    <tbody>
    <tr v-for="item in todoArray" style="background-color: #fafafa;" :key="item.id">
      <td style="padding: 10px; border: 1px solid #ccc;">{{item.text}}</td>
      <td style="padding: 10px; border: 1px solid #ccc;">{{item.description}}</td>
      <td style="padding: 10px; border: 1px solid #ccc;">{{item.completed}}</td>
      <td style="padding: 10px; border: 1px solid #ccc;"><button @click="deleteItem(item)">Delete</button></td>
      <td style="padding: 10px; border: 1px solid #ccc;"><button @click="handleEdit(item)">Edit</button></td>
    </tr>
    </tbody>
  </table>
  <div style="display:flex;">
    <div v-if="!isEditing">
      <div style="display:flex; justify-content: space-between; flex-direction:column; width:100%; margin:auto;">
        <input v-model="itemText" placeholder="Enter Text" style="padding:10px; border-radius:5px; outline:none; border:1px solid #ccc;">
        <input v-model="itemDescription" placeholder="Enter Description" style="padding:10px; margin-top:10px; border-radius:5px; outline:none; border:1px solid #ccc;">
        <button  style="outline:none; border:none; background-color:crimson; color:white; border-radius:5px; padding: 10px; margin-top:10px;" @click="handleToDo">Add Item To The List !</button>
      </div>
    </div>
    <div v-if="isEditing">
      <div style="display:flex; justify-content: space-between; flex-direction:column; width:100%; margin:auto;">
        <input v-model="editedTextItem" placeholder="Enter Text" style="padding:10px; border-radius:5px; outline:none; border:1px solid #ccc;">
        <input v-model="editedDescriptionItem" placeholder="Enter Description" style="padding:10px; margin-top:10px; border-radius:5px; outline:none; border:1px solid #ccc;">
        <input v-model="completedItem" placeholder="Completed?" style="padding:10px; margin-top:10px; border-radius:5px; outline:none; border:1px solid #ccc;">
        <button style="outline:none; border:none; background-color:crimson; color:white; border-radius:5px; padding: 10px; margin-top:10px;" @click="editItem">Edit Item !</button>
      </div>
    </div>
  </div>



</template>

<script setup>

import {ref} from "vue";
const todoArray  = ref([{ id : 1, text:'Text' , description :'Text 1 Description', completed: 'Yes'}, { id : 2, text:'Text 2' , description :'Text 2 Description', completed: 'No'}]);
const itemText = ref('');
const itemDescription = ref('');
const isEditing = ref(false);
const editedTextItem =ref(null);
const editedDescriptionItem = ref(null);
const completedItem = ref ('');
const itemId = ref(null);
// Performing Adding the To-do Item.
function handleToDo(){
  const todoItem = {
    id:todoArray.value.length + 1,
    text:itemText.value,
    description:itemDescription.value,
    completed:false,
  }
  todoArray.value.push(todoItem);
  itemText.value = '';
  itemDescription.value = '';
}
// Getting the object to edit
function handleEdit(item){
  console.log('Item !!!', item )
  console.log('Object : ' , todoArray);
  isEditing.value = true;
  itemId.value = item.id;
  editedTextItem.value = item.text;
  editedDescriptionItem.value = item.description;

}

// Performing the Edit.
function editItem() {
  const itemIndex = todoArray.value.findIndex(item => item.id === itemId.value);

  if (itemIndex !== -1) {
    todoArray.value[itemIndex] = {
      ...todoArray.value[itemIndex],
      text: editedTextItem,
      description: editedDescriptionItem,
      completed: completedItem,
    };

    console.log('Updated ITEM :', todoArray.value[itemIndex]);
    isEditing.value = false;
  } else {
    console.log('Item not found!');
  }
}


// Deleting the To-do Item

function deleteItem(item) {
  console.log('Delete :', item);
 // We have to find the index
    const itemIndex = todoArray.value.findIndex(item => item.id === item.id);
    todoArray.value.splice(itemIndex, 1);
    console.log('Item removed');
}
</script>
