<template>
    <main class="flex justify-center w-full h-screen">
        <div>
            <form class="bg-gray-100 border-black rounded-lg border-2 px-12" method="post">
                <table>
                    <h2 class="text-teal-900 text-xl font-bold pt-6">Employee Management </h2>
                    <hr />
                    <br />
                    <label class="pt-10 py-10" for="book-name">Emp_FirstName:</label><br />
                    <input type="text" id="book-name" name="emp_name" ref="emp-name" v-model="empp.mylist.Emp_FirstName"
                        placeholder="Enter your First name" /><br /><br />
                    <label for="emp-lname">Emp_LastName:</label><br />
                    <input type="text" id="emp-lname" name="emp-lname" placeholder="Enter last name"
                        v-model="empp.mylist.Emp_LastName" /><br /><br />
                    <label for="emp-email">Emp_Email: </label><br />
                    <input type="text" id="emp-email" name="emp-email" placeholder="Enter your email"
                        v-model="empp.mylist.Emp_Email" />
                    <br /><br />
                    <label for="emp-address">Emp_Add: </label><br />
                    <input type="text" id="emp-address" name="emp-address" v-model="empp.mylist.Emp_Add"
                        placeholder="Enter your Addres" />
                    <br /><br />
                    <label for="emp-gender">Emp_Gender: </label><br />
                    <input type="text" id="emp-gender" name="emp-gender" placeholder="Enter your Gender"
                        v-model="empp.mylist.Emp_Gender" />
                    <br /><br />
                    <!-- <label for="employee-image">Emp_profile: </label><br />
                    <input type="file" id="book-image" name="employee-image" /> -->
                    <br /><br />
                    <div>
                        <button
                            class="py-1 px-5 mr-5 bg-blue-500 hover:bg-blue-700 text-white font-bold text-center rounded-md mb-3"
                            type="button" @click="Submit()">
                            Submit
                        </button>
                        <button
                            class="py-1 px-5 bg-blue-500 hover:bg-blue-700 text-white font-bold text-center rounded-md mb-3"
                            type="reset">
                            Reset
                        </button>
                    </div>
                </table>
            </form>
            <button
                class="py-1 px-5 mr-5 bg-blue-500 hover:bg-blue-700 text-white font-bold text-center rounded-md mb-3"
                type="submit">
                GetData
            </button>

            <br />

            <table class="list">
                <tr>
                    <th class="px-4 border-black rounded-lg border-2">id</th>
                    <th class="px-4 border-black rounded-lg border-2">Emp_FirstName</th>
                    <th class="px-4 border-black rounded-lg border-2">Emp_LastName</th>
                    <th class="px-4 border-black rounded-lg border-2">Emp_Email</th>
                    <th class="px-4 border-black rounded-lg border-2">Emp_Add</th>
                    <th class="px-4 border-black rounded-lg border-2">Emp_Gender</th>
                    <!-- <th class="px-4 border-black rounded-lg border-2">Emp_profile</th> -->
                    <th class="px-4 border-black rounded-lg border-2">Action</th>
                </tr>
                <tr v-for="item of empp.allEmp" :key="item.id">

                    <td class="px-4 border-black rounded-lg border-2">
                        {{ item.id }}
                    </td>
                    <td class="px-4 border-black rounded-lg border-2">
                        {{ item.Emp_FirstName }}
                    </td>
                    <td class="px-4 border-black rounded-lg border-2">
                        {{ item.Emp_LastName }}
                    </td>
                    <td class="px-4 border-black rounded-lg border-2">
                        {{ item.Emp_Email }}
                    </td>
                    <td class="px-4 border-black rounded-lg border-2">
                        {{ item.Emp_Add }}
                    </td>
                    <td class="px-4 border-black rounded-lg border-2">
                        {{ item.Emp_Gender }}
                    </td>
                    <!-- <td class="px-4 border-black rounded-lg border-2">
                        <img src="" alt="not found" style="width:100px;height:100px">{{ item.Emp_profile }}
                    </td> -->
                    <td class="px-4 border-black rounded-lg border-2">
                        {{ item.Action }}
                        <button class="mx-3" @click="onDelete(item.id)">Delete</button>
                        <button class="mx-3" @click="onEdit(item.id)">Edit</button>
                    </td>
                </tr>
            </table>
        </div>
    </main>
</template>

<script setup lang="ts">
const { data: count } = await useFetch('http://localhost:8080/Employeee/')
let emp: any = count;

let empp = reactive({
    allEmp: [],
    mylist: {
        id: null,
        Emp_FirstName: "",
        Emp_LastName: "",
        Emp_Gender: "",
        Emp_Email: "",
        Emp_Add: "",
        //Emp_profile: "",
    },
});


getApi();

async function getApi() {
    empp.allEmp = await $fetch('http://localhost:8080/Employeee');
}
//create new employee
// POST API
async function Submit() {

    // const sampleData = {
    //     "id": empp.allEmp.length,
    //     "Emp_FirstName": "sai" + empp.allEmp.length,
    //     "Emp_LastName": "ankita" + empp.allEmp.length,
    //     "Emp_Email": "manki@fg.com" + empp.allEmp.length,
    //     "Emp_Add": "pund" + empp.allEmp.length,
    //     "Emp_Gender": "M" + empp.allEmp.length,
    //     "Emp_profile": "" + empp.allEmp.length,


    // };
    await $fetch('http://localhost:8080/Employeee/', {
        method: 'POST',
        body: (empp.mylist),
    });
    getApi();
}
// PATCH API
async function onEdit(id: number) {
    // const sampleData = {
    //     "id": id,
    //     "Emp_FirstName": "sai" + empp.allEmp.length,
    //     "Emp_LastName": "ankita" + empp.allEmp.length,
    //     "Emp_Email": "sai@gmai.com" + empp.allEmp.length,
    //     "Emp_Add": "ghjgj" + empp.allEmp.length,
    //     "Emp_Gender": "M" + empp.allEmp.length,
    //     "Emp_profile": "" + empp.allEmp.length,

    // };
    const response = await $fetch('http://localhost:8080/Employeee/' + id,
        {
            method: "PATCH",
            body: (empp.mylist),
        }
    );
    getApi();
}
// Delete API
async function onDelete(id: number) {
    await $fetch('http://localhost:8080/Employeee/' + id, {
        method: 'DELETE'
    });
    getApi();
}
</script>
