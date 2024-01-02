<template>
  <div>
    <h1>Quản lí khách hàngr</h1>

    <!-- Button to open the form for adding a new Customer -->
    <button @click="openAddForm">Add Customer</button>

    <!-- Display Customer list -->
    <table>
      <!-- Table header -->
      <thead>
        <tr>
          <th>ID</th>
          <th>Full Name</th>
          <th>Email</th>
          <th>Password</th>
          <th>Phone</th>
          <th>Gender</th>
          <th>Address</th>
          <th>Birthdate</th>
          <th>Point</th>
          <th>Action</th>
        </tr>
      </thead>
      <!-- Table body -->
      <tbody>
        <tr v-for="Customer in Customers" :key="Customer.MANV">
          <td>{{ Customer.MAKH }}</td>
          <td>{{ Customer.HOTEN }}</td>
          <td>{{ Customer.EMAIL }}</td>
          <td>{{ Customer.PASSWORD }}</td>
          <td>{{ Customer.SDT }}</td>
          <td>{{ Customer.GIOITINH === 1 ? "Nam" : "Nữ" }}</td>
          <td>{{ Customer.DIACHI }}</td>
          <td>{{ Customer.NGAYSINH }}</td>
          <td>{{Customer.TICHDIEM}}</td>
          <td>
            <button @click="openEditForm(Customer)">Edit</button>
            <button @click="deleteCustomer(Customer.MAKH)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- Form for adding/editing Customers -->
    <div v-if="showForm">
      <h2>{{ isEditMode ? "Edit Customer" : "Add Customer" }}</h2>
      <form @submit.prevent="saveCustomer">
        <label for="hoten">Full Name:</label>
        <input v-model="CustomerForm.HOTEN" type="text" id="hoten" required />

        <label for="email">Email:</label>
        <input v-model="CustomerForm.EMAIL" type="email" id="email" required />

        <label for="password">Password:</label>
        <input
          v-model="CustomerForm.PASSWORD"
          type="password"
          id="password"
          required
        />

        <label for="sdt">Phone:</label>
        <input v-model="CustomerForm.SDT" type="tel" id="sdt" required />

        <label for="gioitinh">Gender:</label>
        <select v-model="CustomerForm.GIOITINH" id="gioitinh" required>
          <option value="1">Nam</option>
          <option value="0">Nữ</option>
        </select>

        <label for="diachi">Address:</label>
        <input v-model="CustomerForm.DIACHI" type="text" id="diachi" required />

        <label for="ngaysinh">Birthdate:</label>
        <input
          v-model="CustomerForm.NGAYSINH"
          type="date"
          id="ngaysinh"
          required
        />

        <!-- Add more input fields as needed -->

        <button type="submit">{{ isEditMode ? "Update" : "Add" }}</button>
        <button @click="closeForm">Cancel</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      Customers: [],
      showForm: false,
      isEditMode: false,
      CustomerForm: {
        HOTEN: "",
        EMAIL: "",
        SDT: "",
        // Add more fields as needed
      },
    };
  },
  methods: {
    // Fetch Customer data from the server
    async fetchCustomers() {
      try {
        const response = await fetch("http://localhost:3000/Customers");
        const data = await response.json();
        this.Customers = data;
      } catch (error) {
        console.error("Error fetching Customer data:", error);
      }
    },

    // Save or update Customer information
    async saveCustomer() {
      try {
        if (this.isEditMode) {
          // Update existing Customer
          await fetch(
            `http://localhost:3000/Customers/${this.CustomerForm.MAKH}`,
            {
              method: "PUT",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify(this.CustomerForm),
            }
          );
        } else {
          // Add new Customer
          await fetch("http://localhost:3000/Customers", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify(this.CustomerForm),
          });
        }

        // Clear the form and fetch updated Customer data
        this.closeForm();
        await this.fetchCustomers();
      } catch (error) {
        console.error("Error saving Customer data:", error);
      }
    },

    // Delete Customer
    async deleteCustomer(CustomerId) {
      try {
        await fetch(`http://localhost:3000/Customers/${CustomerId}`, {
          method: "DELETE",
        });

        // Fetch updated Customer data after deletion
        this.fetchCustomers();
      } catch (error) {
        console.error("Error deleting Customer:", error);
      }
    },

    // Open the form for adding a new Customer
    openAddForm() {
      this.showForm = true;
      this.isEditMode = false;
      this.CustomerForm = {
        HOTEN: "",
        EMAIL: "",
        SDT: "",
        // Initialize other fields as needed
      };
    },

    // Open the form for editing an existing Customer
    openEditForm(Customer) {
      this.showForm = true;
      this.isEditMode = true;
      this.CustomerForm = { ...Customer };
    },

    // Close the form
    closeForm() {
      this.showForm = false;
      this.isEditMode = false;
      this.CustomerForm = {
        HOTEN: "",
        EMAIL: "",
        SDT: "",
        // Clear other fields as needed
      };
    },
  },
  mounted() {
    // Fetch initial Customer data when the component is mounted
    this.fetchCustomers();
  },
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

thead {
  background-color: #f2f2f2;
}

th,
td {
  padding: 10px;
  border: 1px solid #ddd;
  text-align: left;
}

th {
  font-weight: bold;
}

/* CSS cho form */
form {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  margin: 20px 0;
}
</style>
