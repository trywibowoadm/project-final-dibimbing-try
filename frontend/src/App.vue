<template>
    <v-card>
        <div v-if="authorization">
            <v-layout>
                <v-app-bar
                    class="bg-blue-grey-lighten-5"
                    prominent
                >
                    <v-toolbar-title><v-img
                        :width="25"
                        aspect-ratio="16/9"
                        cover
                        src="melawai.ico"
                    ></v-img>CRM Melawai</v-toolbar-title>
                    <v-spacer></v-spacer>
                    <v-btn 
                        variant="text"
                        icon="mdi-logout"
                        @click="handleLogout"
                    />
                </v-app-bar>
                <v-navigation-drawer
                    expand-on-hover
                    rail
                >
                    <v-list>
                        <v-list-item
                            prepend-avatar="melawai.ico"
                            :title="developer.name"
                            :subtitle="developer.email"
                        ></v-list-item>
                    </v-list>
                    <v-divider></v-divider>
                    <v-list density="compact" nav>
                        <v-list-item
                            v-for="(link, i) in links"
                            :key="i"
                            :to="link.to"
                            :prepend-icon="link.icon"
                            :title="link.title"
                            :value="i"
                        />
                    </v-list>
                </v-navigation-drawer>
                <v-main style="min-height: 100vh; max-height: 100%;">
                    <div style="margin: 40px 40px 40px 40px">
                        <router-view />
                    </div>
                </v-main>
            </v-layout>
        </div>
        <div v-else style="min-height: 100vh; max-height: 100%;">
            <LoginComponent />
        </div>
    </v-card>
</template>

<script>
    import Login from './views/Auth/Login.vue'
    import { sendRequest } from './utils/request'

    export default {
        components: {
            LoginComponent: Login
        },
        data() {
            return {
                authorization: localStorage.getItem('token') ? true : false,
                links: [
                    {
                        to: "/customers",
                        icon: "mdi-account-group",
                        title: "Customers",
                    },
                    {
                        to: "/customer-receipts",
                        icon: "mdi-receipt-text-check",
                        title: "Customer Receipts",
                    },
                    {
                        to: "/materials",
                        icon: "mdi-glasses",
                        title: "Materials",
                    },
                    {
                        to: "/sales-order",
                        icon: "mdi-cart-variant",
                        title: "Sales Order",
                    },
                ],
                developer: {
                    name: import.meta.env.VITE_APP_DEVELOPER,
                    email: import.meta.env.VITE_APP_DEVELOPER_EMAIL
                }
            }
        },
        methods: {
            handleLogout() {
                sendRequest('POST', `${import.meta.env.VITE_APP_BACKEND_HOST}/api/auth/logout`)
                .then(res => {
                    localStorage.removeItem('token')
                    window.location.href = "/"
                })
                .catch(err => {
                    console.log(err)
                })
            }
        }
    }
</script>