<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <table class="table table-bordered mt-5">
          <thead>
            <tr>
              <th>TO DO</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in miniList" :key="index">
              <td class="align-middle w-75">
                {{ 
                  `( ${item.product_type} ) ${item.style_name} -- ${ getBenefitsDescriptions(item.benefits) }` 
                }}
              </td>
              <td class="align-middle text-center w-75">
                <button
                  class="btn btn-info btn-sm mx-1"
                  @click="handleEdit(item.id)"
                >
                  Edit
                </button>
                <button
                  class="btn btn-danger btn-sm mx-1"
                  @click="handleDelete(item.id)"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
          <tfoot>
            <td class="align-middle text-center w-75">
              <div class="form-group">
                <label for="">
                  {{ editMode ? "Edit" : "Add" }}
                </label>
                <input
                  type="text"
                  class="form-control"
                  v-model="benefit.product_Type"
                />
              </div>
              <div class="form-group">
                <label for="">
                  {{ editMode ? "Edit" : "Add" }}
                </label>
                <input
                  type="text"
                  class="form-control"
                  v-model="benefit.product_Type"
                />
              </div>
              <div class="form-group">
                <label for="">
                  {{ editMode ? "Edit" : "Add" }}
                </label>
                <input
                  type="text"
                  class="form-control"
                  v-model="benefit.product_Type"
                />
              </div>
              <div class="form-group">
                <label for="">
                  {{ editMode ? "Edit" : "Add" }}
                </label>
                <input
                  type="text"
                  class="form-control"
                  v-model="benefit.product_Type"
                />
              </div>
            </td>
            <td class="align-middle text-center w-25">
              <button 
                class="btn btn-info btn-sm mx-1"
                @click="handlebenefit"
              >
                {{ editMode ? "Edit" : "Add" }}
              </button>
              <button
                v-if="editMode"
                class="btn btn-danger btn-sm mx-1"
                @click="handleCancel"
              >
                Cancel
              </button>
            </td>
          </tfoot>
        </table>
        <div>
          <button
              class="btn btn-success btn-sm mx-1"
              @click="handleLanguage()"
            >
              Language
          </button>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
  import Axios from "axios";
  // const url = "http://localhost:3500/todo";
  const url = "http://localhost:5001/api/benefits";

  export default {
    data() {
      return {
        benefitList: [],
        benefit: {},
        miniList: [],
        mini: {},
        editMode: false,
        language: 9
      };
    },
    methods: {
      getBenefitsDescriptions(benefits) {
        // var descriptions = item.benefits
        //   .map(benefit => 
        //   benefit.descriptions
        //   .filter(desc => 
        //     desc.language == this.language)
        //     .map(x => x.description))

        // return descriptions;
        return benefits.map(b => b.descriptions.filter(d => d.language == this.language)
                .map(d => d.description))
      },
      handleEdit(id) {
        this.editMode = true;
        this.benefit = this.benefitList.find((item) => item.id == id);
      },
      handleCancel() {
        this.editMode = false;
        this.benefit = "";
      },
      handleLanguage() {
        this.language = this.language != 9 ? 9 : 12;
      },
      async handlebenefit() {
        const id = this.benefit.id;

        if (this.editMode) {
            await Axios.put(`${url}/${id}`, this.benefit);
            this.editMode = false;
            this.benefit.product_type = "";
        } else {
            await Axios.post(url, this.benefit);
            this.benefit.product_type = "";
        }
          Axios.get(url).then(
            (response) => (this.benefitList = response.data)
          );
      },
      async handleDelete(id) {
        await Axios.delete(`${url}/${id}`);
        Axios.get(url).then(
          (response) => (this.benefitList = response.data)
        );
      }
    },
    created() {
      // Axios.get('http://localhost:5001/api/benefits').then((response) => (this.benefitList = response.data));
      Axios.get('http://localhost:5001/api/productminis', { headers:  { "Accept-Language": "all" }}).then((response) => (this.miniList = response.data));
      console.log(this.miniList);
    },
    mounted() {
      // console.log(this.miniList);
    }

  };

</script>


 