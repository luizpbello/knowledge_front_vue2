<template>
	<div id="app" :class="{'hide-menu' : !isMenuVisible || !user}">
		<HeaderApp title="Bello - Base de Conhecimento"
			:hideToggle="!user"
			:hideUserDropdown="!user" />
		<MenuApp v-if="user" />
		<LoadingApp v-if="validatingToken" />
		<ContentApp v-else />
		<FooterApp />
	</div>
</template>

<script>
import { mapState } from 'vuex'
import HeaderApp from './components/template/Header.vue'
import FooterApp from './components/template/Footer.vue'
import MenuApp from './components/template/Menu.vue'
import ContentApp from './components/template/ContentApp.vue'
import axios from 'axios'
import { userKey } from '@/global'
import { baseApiUrl } from '@/global'
import LoadingApp from './components/template/Loading'

export default {
	name: "App",
	components: { HeaderApp, FooterApp, MenuApp, ContentApp, LoadingApp },
	computed: mapState(['isMenuVisible', 'user']),
	data () {
		return{
			validatingToken: true
		}
	},
	methods: {
		async validateToken() {
			this.validatingToken = true

			const json = localStorage.getItem(userKey)
			const userData = JSON.parse(json)
			this.$store.commit('setUser', null)

			if(!userData) {
					this.validatingToken = false
					this.$router.push({ name: 'auth' })
					return
			}

			const res = await axios.post(`${baseApiUrl}/validateToken`, userData)		
			
			if (res.data) {
				this.$store.commit('setUser', userData)
				if(this.$mq == 'xs' || this.$mq =='sm'){
                    this.$store.commit('toggleMenu', false)
                }
			} else {
				localStorage.removeItem(userKey)
				this.$router.push({ name: 'auth' })
			}
			this.validatingToken = false
		}
	},
	created() {
		this.validateToken()
	}
}
</script>

<style>
	* {
		font-family: 'Roboto', sans-serif;
	}

	body{
		margin: 0;
	}

	#app {
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;

		height: 100vh;
		display: grid;
		grid-template-rows: 60px 1fr 40px;
		grid-template-columns: 300px 1fr;
		grid-template-areas:
			"header header"
			"menu content"
			"menu footer";
	}

	#app.hide-menu{
		grid-template-areas:
		"header header"
		"content content"
		"footer footer"
		;
	}

</style>