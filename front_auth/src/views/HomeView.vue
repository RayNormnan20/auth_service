<script setup>
import { ref, computed } from "vue";
import axios from "axios";
import SidebarComponent from "./../components/SidebarComponent.vue";
import TopbarComponent from "./../components/TopbarComponent.vue";

const errorMessage = ref("");
const user = ref("");
const isLoading = ref(false);
const logged = ref(false);

// URL del endpoint de login
const meUrl = "/v1/auth/me";
// Configuración del cliente Axios
const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_AUTH_URL, // URL base de la API
  headers: {
    "Content-Type": "application/json",
  },
});
// Agregar un interceptor para incluir el token en todas las solicitudes
apiClient.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem("token");
    const tokenType = localStorage.getItem("token_type");

    if (token && tokenType) {
      config.headers.Authorization = `${tokenType} ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

// Función para obtener el perfil de usuario
const getUserProfile = async () => {
  try {
    const response = await apiClient.get(meUrl); // No es necesario repetir el encabezado aquí
    localStorage.setItem("user", JSON.stringify(response.data));
    logged.value = true;
    user.value = response.data;
    return response.data;
  } catch (error) {
    localStorage.clear();

    // Redirigir al usuario
    window.location.href = "/";
    isLoading.value = false;
    console.error(
      "Error al obtener perfil:",
      error.response?.data || error.message
    );
    throw error;
  }
};


getUserProfile();
</script>
<template>
  <!-- ============================================================== -->
  <!-- Main wrapper - style you can find in pages.scss -->
  <!-- ============================================================== -->
  <div
    v-show="logged"
    id="main-wrapper"
    data-theme="light"
    data-layout="vertical"
    data-navbarbg="skin6"
    data-sidebartype="full"
    data-sidebar-position="fixed"
    data-header-position="fixed"
    data-boxed-layout="full"
  >
    
  <TopbarComponent/>
    <!-- ============================================================== -->
    <!-- End Topbar header -->
    <!-- ============================================================== -->
    <!-- ============================================================== -->
    <!-- Left Sidebar - style you can find in sidebar.scss  -->
    <!-- ============================================================== -->
    <SidebarComponent/>
    <!-- ============================================================== -->
    <!-- End Left Sidebar - style you can find in sidebar.scss  -->
    <!-- ============================================================== -->
    <!-- ============================================================== -->
    <!-- Page wrapper  -->
    <!-- ============================================================== -->
    <div class="page-wrapper">
      <!-- ============================================================== -->
      <!-- Bread crumb and right sidebar toggle -->
      <!-- ============================================================== -->
      <div class="page-breadcrumb">
        <div class="row">
          <div class="col-7 align-self-center">
            <h3
              class="page-title text-truncate text-dark font-weight-medium mb-1"
            >
              Good Morning Jason!
            </h3>
            <div class="d-flex align-items-center">
              <nav aria-label="breadcrumb">
                <ol class="breadcrumb m-0 p-0">
                  <li class="breadcrumb-item">
                    <a href="index.html">Dashboard</a>
                  </li>
                </ol>
              </nav>
            </div>
          </div>
          <div class="col-5 align-self-center">
            <div class="customize-input float-end">
              <select
                class="custom-select custom-select-set form-control bg-white border-0 custom-shadow custom-radius"
              >
                <option selected>Aug 23</option>
                <option value="1">July 23</option>
                <option value="2">Jun 23</option>
              </select>
            </div>
          </div>
        </div>
      </div>
      <!-- ============================================================== -->
      <!-- End Bread crumb and right sidebar toggle -->
      <!-- ============================================================== -->
      <!-- ============================================================== -->
      <!-- Container fluid  -->
      <!-- ============================================================== -->
      <div class="container-fluid">
        <!-- *************************************************************** -->
        <!-- Start First Cards -->
        <!-- *************************************************************** -->
        <div class="row">
          <div class="col-sm-6 col-lg-3">
            <div class="card border-end">
              <div class="card-body">
                <div class="d-flex align-items-center">
                  <div>
                    <div class="d-inline-flex align-items-center">
                      <h2 class="text-dark mb-1 font-weight-medium">236</h2>
                      <span
                        class="badge bg-primary font-12 text-white font-weight-medium rounded-pill ms-2 d-lg-block d-md-none"
                        >+18.33%</span
                      >
                    </div>
                    <h6
                      class="text-muted font-weight-normal mb-0 w-100 text-truncate"
                    >
                      New Clients
                    </h6>
                  </div>
                  <div class="ms-auto mt-md-3 mt-lg-0">
                    <span class="opacity-7 text-muted"
                      ><i data-feather="user-plus"></i
                    ></span>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-sm-6 col-lg-3">
            <div class="card border-end">
              <div class="card-body">
                <div class="d-flex align-items-center">
                  <div>
                    <h2
                      class="text-dark mb-1 w-100 text-truncate font-weight-medium"
                    >
                      <sup class="set-doller">$</sup>18,306
                    </h2>
                    <h6
                      class="text-muted font-weight-normal mb-0 w-100 text-truncate"
                    >
                      Earnings of Month
                    </h6>
                  </div>
                  <div class="ms-auto mt-md-3 mt-lg-0">
                    <span class="opacity-7 text-muted"
                      ><i data-feather="dollar-sign"></i
                    ></span>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-sm-6 col-lg-3">
            <div class="card border-end">
              <div class="card-body">
                <div class="d-flex align-items-center">
                  <div>
                    <div class="d-inline-flex align-items-center">
                      <h2 class="text-dark mb-1 font-weight-medium">1538</h2>
                      <span
                        class="badge bg-danger font-12 text-white font-weight-medium rounded-pill ms-2 d-md-none d-lg-block"
                        >-18.33%</span
                      >
                    </div>
                    <h6
                      class="text-muted font-weight-normal mb-0 w-100 text-truncate"
                    >
                      New Projects
                    </h6>
                  </div>
                  <div class="ms-auto mt-md-3 mt-lg-0">
                    <span class="opacity-7 text-muted"
                      ><i data-feather="file-plus"></i
                    ></span>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-sm-6 col-lg-3">
            <div class="card">
              <div class="card-body">
                <div class="d-flex align-items-center">
                  <div>
                    <h2 class="text-dark mb-1 font-weight-medium">864</h2>
                    <h6
                      class="text-muted font-weight-normal mb-0 w-100 text-truncate"
                    >
                      Projects
                    </h6>
                  </div>
                  <div class="ms-auto mt-md-3 mt-lg-0">
                    <span class="opacity-7 text-muted"
                      ><i data-feather="globe"></i
                    ></span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- *************************************************************** -->
        <!-- End First Cards -->
       
        <!-- *************************************************************** -->
        <!-- End Location and Earnings Charts Section -->
        <!-- *************************************************************** -->
        <!-- *************************************************************** -->
        <!-- Start Top Leader Table -->
        <!-- *************************************************************** -->
        <div class="row">
          <div class="col-12">
            <div class="card">
              <div class="card-body">
                <div class="d-flex align-items-center mb-4">
                  <h4 class="card-title">Top Leaders</h4>
                  <div class="ms-auto">
                    <div class="dropdown sub-dropdown">
                      <button
                        class="btn btn-link text-muted dropdown-toggle"
                        type="button"
                        id="dd1"
                        data-bs-toggle="dropdown"
                        aria-haspopup="true"
                        aria-expanded="false"
                      >
                        <i data-feather="more-vertical"></i>
                      </button>
                      <div
                        class="dropdown-menu dropdown-menu-right"
                        aria-labelledby="dd1"
                      >
                        <a class="dropdown-item" href="#">Insert</a>
                        <a class="dropdown-item" href="#">Update</a>
                        <a class="dropdown-item" href="#">Delete</a>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="table-responsive">
                  <table class="table no-wrap v-middle mb-0">
                    <thead>
                      <tr class="border-0">
                        <th
                          class="border-0 font-14 font-weight-medium text-muted"
                        >
                          Team Lead
                        </th>
                        <th
                          class="border-0 font-14 font-weight-medium text-muted px-2"
                        >
                          Project
                        </th>
                        <th
                          class="border-0 font-14 font-weight-medium text-muted"
                        >
                          Team
                        </th>
                        <th
                          class="border-0 font-14 font-weight-medium text-muted text-center"
                        >
                          Status
                        </th>
                        <th
                          class="border-0 font-14 font-weight-medium text-muted text-center"
                        >
                          Weeks
                        </th>
                        <th
                          class="border-0 font-14 font-weight-medium text-muted"
                        >
                          Budget
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td class="border-top-0 px-2 py-4">
                          <div class="d-flex no-block align-items-center">
                            <div class="me-3">
                              <img
                                src="/assets/images/users/widget-table-pic1.jpg"
                                alt="user"
                                class="rounded-circle"
                                width="45"
                                height="45"
                              />
                            </div>
                            <div class="">
                              <h5
                                class="text-dark mb-0 font-16 font-weight-medium"
                              >
                                Hanna Gover
                              </h5>
                              <span class="text-muted font-14"
                                >hgover@gmail.com</span
                              >
                            </div>
                          </div>
                        </td>
                        <td class="border-top-0 text-muted px-2 py-4 font-14">
                          Elite Admin
                        </td>
                        <td class="border-top-0 px-2 py-4">
                          <div class="popover-icon">
                            <a
                              class="btn btn-primary rounded-circle btn-circle font-12"
                              href="javascript:void(0)"
                              >DS</a
                            >
                            <a
                              class="btn btn-danger rounded-circle btn-circle font-12 popover-item"
                              href="javascript:void(0)"
                              >SS</a
                            >
                            <a
                              class="btn btn-cyan rounded-circle btn-circle font-12 popover-item"
                              href="javascript:void(0)"
                              >RP</a
                            >
                            <a
                              class="btn btn-success text-white rounded-circle btn-circle font-20"
                              href="javascript:void(0)"
                              >+</a
                            >
                          </div>
                        </td>
                        <td class="border-top-0 text-center px-2 py-4">
                          <i
                            class="fa fa-circle text-primary font-12"
                            data-bs-toggle="tooltip"
                            data-placement="top"
                            title="In Testing"
                          ></i>
                        </td>
                        <td
                          class="border-top-0 text-center font-weight-medium text-muted px-2 py-4"
                        >
                          35
                        </td>
                        <td
                          class="font-weight-medium text-dark border-top-0 px-2 py-4"
                        >
                          $96K
                        </td>
                      </tr>
                      <tr>
                        <td class="px-2 py-4">
                          <div class="d-flex no-block align-items-center">
                            <div class="me-3">
                              <img
                                src="/assets/images/users/widget-table-pic2.jpg"
                                alt="user"
                                class="rounded-circle"
                                width="45"
                                height="45"
                              />
                            </div>
                            <div class="">
                              <h5
                                class="text-dark mb-0 font-16 font-weight-medium"
                              >
                                Daniel Kristeen
                              </h5>
                              <span class="text-muted font-14"
                                >Kristeen@gmail.com</span
                              >
                            </div>
                          </div>
                        </td>
                        <td class="text-muted px-2 py-4 font-14">
                          Real Homes WP Theme
                        </td>
                        <td class="px-2 py-4">
                          <div class="popover-icon">
                            <a
                              class="btn btn-primary rounded-circle btn-circle font-12"
                              href="javascript:void(0)"
                              >DS</a
                            >
                            <a
                              class="btn btn-danger rounded-circle btn-circle font-12 popover-item"
                              href="javascript:void(0)"
                              >SS</a
                            >
                            <a
                              class="btn btn-success text-white rounded-circle btn-circle font-20"
                              href="javascript:void(0)"
                              >+</a
                            >
                          </div>
                        </td>
                        <td class="text-center px-2 py-4">
                          <i
                            class="fa fa-circle text-success font-12"
                            data-bs-toggle="tooltip"
                            data-placement="top"
                            title="Done"
                          ></i>
                        </td>
                        <td
                          class="text-center text-muted font-weight-medium px-2 py-4"
                        >
                          32
                        </td>
                        <td class="font-weight-medium text-dark px-2 py-4">
                          $85K
                        </td>
                      </tr>
                      <tr>
                        <td class="px-2 py-4">
                          <div class="d-flex no-block align-items-center">
                            <div class="me-3">
                              <img
                                src="/assets/images/users/widget-table-pic3.jpg"
                                alt="user"
                                class="rounded-circle"
                                width="45"
                                height="45"
                              />
                            </div>
                            <div class="">
                              <h5
                                class="text-dark mb-0 font-16 font-weight-medium"
                              >
                                Julian Josephs
                              </h5>
                              <span class="text-muted font-14"
                                >Josephs@gmail.com</span
                              >
                            </div>
                          </div>
                        </td>
                        <td class="text-muted px-2 py-4 font-14">
                          MedicalPro WP Theme
                        </td>
                        <td class="px-2 py-4">
                          <div class="popover-icon">
                            <a
                              class="btn btn-primary rounded-circle btn-circle font-12"
                              href="javascript:void(0)"
                              >DS</a
                            >
                            <a
                              class="btn btn-danger rounded-circle btn-circle font-12 popover-item"
                              href="javascript:void(0)"
                              >SS</a
                            >
                            <a
                              class="btn btn-cyan rounded-circle btn-circle font-12 popover-item"
                              href="javascript:void(0)"
                              >RP</a
                            >
                            <a
                              class="btn btn-success text-white rounded-circle btn-circle font-20"
                              href="javascript:void(0)"
                              >+</a
                            >
                          </div>
                        </td>
                        <td class="text-center px-2 py-4">
                          <i
                            class="fa fa-circle text-primary font-12"
                            data-bs-toggle="tooltip"
                            data-placement="top"
                            title="Done"
                          ></i>
                        </td>
                        <td
                          class="text-center text-muted font-weight-medium px-2 py-4"
                        >
                          29
                        </td>
                        <td class="font-weight-medium text-dark px-2 py-4">
                          $81K
                        </td>
                      </tr>
                      <tr>
                        <td class="border-bottom-0 px-2 py-4">
                          <div class="d-flex no-block align-items-center">
                            <div class="me-3">
                              <img
                                src="/assets/images/users/widget-table-pic4.jpg"
                                alt="user"
                                class="rounded-circle"
                                width="45"
                                height="45"
                              />
                            </div>
                            <div class="">
                              <h5
                                class="text-dark mb-0 font-16 font-weight-medium"
                              >
                                Jan Petrovic
                              </h5>
                              <span class="text-muted font-14"
                                >hgover@gmail.com</span
                              >
                            </div>
                          </div>
                        </td>
                        <td
                          class="border-bottom-0 text-muted px-2 py-4 font-14"
                        >
                          Hosting Press HTML
                        </td>
                        <td class="border-bottom-0 px-2 py-4">
                          <div class="popover-icon">
                            <a
                              class="btn btn-primary rounded-circle btn-circle font-12"
                              href="javascript:void(0)"
                              >DS</a
                            >
                            <a
                              class="btn btn-success text-white font-20 rounded-circle btn-circle"
                              href="javascript:void(0)"
                              >+</a
                            >
                          </div>
                        </td>
                        <td class="border-bottom-0 text-center px-2 py-4">
                          <i
                            class="fa fa-circle text-danger font-12"
                            data-bs-toggle="tooltip"
                            data-placement="top"
                            title="In Progress"
                          ></i>
                        </td>
                        <td
                          class="border-bottom-0 text-center text-muted font-weight-medium px-2 py-4"
                        >
                          23
                        </td>
                        <td
                          class="border-bottom-0 font-weight-medium text-dark px-2 py-4"
                        >
                          $80K
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- *************************************************************** -->
        <!-- End Top Leader Table -->
        <!-- *************************************************************** -->
      </div>
      <!-- ============================================================== -->
      <!-- End Container fluid  -->
      <!-- ============================================================== -->
      <!-- ============================================================== -->
      <!-- footer -->
      <!-- ============================================================== -->
      <footer class="footer text-center text-muted">
        Diseñado por Chris Gámez |
        <a href="https://agenciati.com/">Agencia TI</a>.
      </footer>
      <!-- ============================================================== -->
      <!-- End footer -->
      <!-- ============================================================== -->
    </div>
    <!-- ============================================================== -->
    <!-- End Page wrapper  -->
    <!-- ============================================================== -->
  </div>
  <div class="text-center" v-show="!logged">
    <button class="btn btn-primary" type="button" disabled="">
      <span
        class="spinner-border spinner-border-sm"
        role="status"
        aria-hidden="true"
      ></span>
      Espere...
    </button>
  </div>
</template>

<style lang="css" scoped></style>
