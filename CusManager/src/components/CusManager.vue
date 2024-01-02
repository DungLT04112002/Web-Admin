<template>
  <div>
    <div class="form-container">
      <fieldset class="form-cus">
        <h3>Thông tin khách hàng</h3>

        <div class="form-group">
          <label for="fullname">Họ và tên</label>
          <input
            type="text"
            id="fullname"
            v-model="fullname"
            @input="resetError('fullname')"
          />
          <span class="error" v-if="!fullname && formSubmitted"
            >Vui lòng nhập họ và tên</span
          >
        </div>

        <div class="form-group">
          <label for="email">Địa chỉ email</label>
          <input
            type="text"
            id="email"
            v-model="email"
            @input="resetError('email')"
          />
          <span
            class="error"
            v-if="!checkEmail(email) && email && formSubmitted"
            >Email sai định dạng</span
          >
        </div>

        <div class="form-group">
          <label for="customerId">Mã khách hàng</label>
          <input
            type="text"
            id="customerId"
            v-model="customerId"
            @input="resetError('customerId')"
          />
          <span class="error" v-if="!customerId && formSubmitted"
            >Vui lòng nhập mã khách hàng</span
          >
        </div>

        <div class="form-group">
          <label for="phone">Số điện thoại</label>
          <input
            type="text"
            id="phone"
            v-model="phone"
            @input="resetError('phone')"
          />
          <span
            class="error"
            v-if="
              (!isValidPhoneNumber(phone) || phone.length !== 10) &&
              phone &&
              formSubmitted
            "
            >Số điện thoại không hợp lệ</span
          >
        </div>

        <div class="form-group">
          <label for="date">Ngày sinh</label>
          <input
            type="text"
            id="date"
            v-model="date"
            @input="resetError('date')"
          />
          <span
            class="error"
            v-if="
              (!isValidDate(date) || !isDateValid(date)) &&
              date &&
              formSubmitted
            "
            >Ngày tháng năm sinh không hợp lệ</span
          >
        </div>

        <div class="form-group">
          <label for="address">Địa chỉ</label>
          <input
            type="text"
            id="address"
            v-model="address"
            @input="resetError('address')"
          />
          <span class="error" v-if="!address && formSubmitted"
            >Vui lòng nhập địa chỉ</span
          >
        </div>

        <div class="form-group">
          <label for="point">Điểm tích lũy</label>
          <input
            type="text"
            id="point"
            v-model="point"
            @input="resetError('point')"
          />
          <span class="error" v-if="isNaN(point) && point && formSubmitted"
            >Điểm tích lũy không hợp lệ</span
          >
        </div>

        <div class="form-group">
          <label for="gender">Giới tính</label>
          <input
            type="radio"
            id="gender"
            name="gender"
            v-model="gender"
            value="1"
          />
          <label for="gender">Nam</label>
          <input
            type="radio"
            id="gender"
            name="gender"
            v-model="gender"
            value="2"
          />
          <label for="gender">Nữ</label>
          <span class="error" v-if="!gender && formSubmitted"
            >Vui lòng chọn giới tính</span
          >
        </div>

        <div class="form-group">
          <button @click="save">Lưu lại</button>
        </div>
      </fieldset>
    </div>

    <div class="list-customer">
      <h3>Thông tin khách hàng</h3>
      <table id="table-customer" border="1" cellspacing="0" cellpadding="0">
        <tr>
          <td>#</td>
          <td>Họ và tên</td>
          <td>Email</td>
          <td>Mã khách hàng</td>
          <td>Số điện thoại</td>
          <td>Ngày sinh</td>
          <td>Địa chỉ</td>
          <td>Điểm tích lũy</td>
          <td>Giới tính</td>
          <td>Hành động</td>
        </tr>
        <tr v-for="(customer, index) in customers" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ customer.fullname }}</td>
          <td>{{ customer.email }}</td>
          <td>{{ customer.customerId }}</td>
          <td>{{ customer.phone }}</td>
          <td>{{ customer.date }}</td>
          <td>{{ customer.address }}</td>
          <td>{{ customer.point }}</td>
          <td>{{ customer.gender === "1" ? "Nam" : "Nữ" }}</td>
          <td>
            <a href="#" @click="editCustomer(index)">Edit</a> |
            <a href="#" @click="deleteCustomer(index)">Delete</a>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formSubmitted: false,
      errors: {
        fullname: false,
        email: false,
        customerId: false,
        phone: false,
        date: false,
        address: false,
        point: false,
        gender: false,
      },
      id: "",
      fullname: "",
      customerId: "",
      phone: "",
      email: "",
      password: "",
      date: "",
      address: "",
      point: "",
      gender: "",
      customers: [],
    };
  },
  methods: {
    checkEmail(email) {
      return /^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$/.test(email);
    },
    isValidPhoneNumber(phone) {
      return /^[0-9]{10}$/.test(phone);
    },
    isValidDate(date) {
      var regexNgayThang = /^\d{2}\/\d{2}\/\d{4}$/;
      return regexNgayThang.test(date);
    },
    isDateValid(date) {
      // Thêm kiểm tra định dạng ngày tháng ở đây
      // ...
      return true;
    },
    save() {
      this.formSubmitted = true;
      this.resetErrors();

      if (this.validateForm()) {
        this.customers.push({
          fullname: this.fullname,
          customerId: this.customerId,
          email: this.email,
          phone: this.phone,
          date: this.date,
          address: this.address,
          point: this.point,
          gender: this.gender,
        });
        this.clearFields();
      }
    },
    validateForm() {
      let isValid = true;

      if (this.fullname === "") {
        this.errors.fullname = true;
        isValid = false;
      }

      if (!this.checkEmail(this.email) || this.email === "") {
        this.errors.email = true;
        isValid = false;
      }

      if (this.customerId === "") {
        this.errors.customerId = true;
        isValid = false;
      }

      if (!this.isValidPhoneNumber(this.phone) || this.phone === "") {
        this.errors.phone = true;
        isValid = false;
      }

      if (
        !this.isValidDate(this.date) ||
        !this.isDateValid(this.date) ||
        this.date === ""
      ) {
        this.errors.date = true;
        isValid = false;
      }

      if (this.address === "") {
        this.errors.address = true;
        isValid = false;
      }

      if (isNaN(this.point) || this.point === "") {
        this.errors.point = true;
        isValid = false;
      }

      if (this.gender === "") {
        this.errors.gender = true;
        isValid = false;
      }

      return isValid;
    },
    resetError(field) {
      this.errors[field] = false;
    },
    resetErrors() {
      for (let key in this.errors) {
        this.errors[key] = false;
      }
    },
    clearFields() {
      this.fullname = "";
      this.customerId = "";
      this.phone = "";
      this.email = "";
      this.date = "";
      this.address = "";
      this.point = "";
      this.gender = "";
      this.formSubmitted = false;
    },
    editCustomer(index) {
      // Thêm logic để chỉnh sửa thông tin khách hàng
      // ...
    },
    deleteCustomer(index) {
      // Thêm logic để xóa thông tin khách hàng
      // ...
    },
    // Các hàm khác
  },
};
</script>

<style scoped>
/* CSS cho form */
.form-container {
  background-color: #f5f5f5;
  padding: 20px;
  border-radius: 10px;
  color: #333;
  margin: 20px auto;
  max-width: 800px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 20px;
  margin-left: 20px;
  margin-right: 20px;
}

.form-group label {
  display: block;
  margin-top: 10px;
  font-weight: bold;
  font-size: 1rem;
}



.form-group input[type="text"],
.form-group input[type="radio"] {
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  width: 100%;
  font-size: 1rem;
}

.form-group input[type="text"]:focus {
  border-color: #ff414d;
}

.error {
  color: #ff414d;
  font-size: 0.8rem;
  margin-top: 5px;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #ff414d;
  border: none;
  border-radius: 5px;
  color: white;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #ff2e3b;
}

/* CSS cho bảng thông tin khách hàng */
.list-customer {
  margin-top: 20px;
}

.list-customer h3 {
  color: #ff414d;
  font-size: 1.2rem;
}

#table-customer {
  width: 100%;
  background-color: #fff;
  border-collapse: collapse;
  color: #333;
}

#table-customer td,
#table-customer th {
  padding: 15px;
  text-align: left;
  font-size: 1rem;
  border: 1px solid #ccc;
}

#table-customer tr:nth-child(even) {
  background-color: #f2f2f2;
}

#table-customer th {
  background-color: #ff414d;
  color: white;
}

a {
  color: #4dc9f6;
  text-decoration: none;
  font-size: 1rem;
}

a:hover {
  text-decoration: underline;
}
</style>
