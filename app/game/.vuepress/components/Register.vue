<template>
	<div class="w-full max-w-xs">
		<h1>Register</h1>
		<form class="bg-white px-8 pt-6 pb-8 mb-4" @submit.prevent="submit">
			<p class="text-red-500 text-xs italic" aria-live="polite">{{ message }}</p>

			<div class="mb-4">
				<label class="mb-2" for="email">Email</label>
				<input
					class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
					id="email"
					type="email"
					v-model="email"
					placeholder="Email"
					required
					tabindex="0"
				/>
			</div>
			<div class="mb-6">
				<label class="mb-2" for="password">Password</label>
				<input
					class="shadow appearance-none border border-red-500 rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
					id="password"
					type="password"
					placeholder="Password"
					v-model="password"
					required
				/>
			</div>
			<div class="flex justify-between">
				<button
					class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 border border-blue-700 rounded"
					type="submit"
				>
					Sign Up
				</button>
			</div>
		</form>

		<router-link class="has-text-grey" to="/pyramid/login">
			<p>Login</p>
		</router-link>
	</div>
</template>
<script>
import axios from 'axios';
import { getUID, setUID, hasUID, setSessionTicket, getSessionTicket } from '@theme/utils/helpers';
export default {
	data() {
		return {
			message: '',
			email: '',
			password: '',
		};
	},
	methods: {
		submit() {
			this.message = '';
			axios
				.post('/api/register', {
					email: this.email,
					password: this.password,
				})
				.then((response) => {
					console.log(response);
					if (response.data.errorMessage == null) {
						setSessionTicket(response.data.SessionTicket);
						this.linkCustomID();
					} else {
						this.message = response.data.errorMessage;
					}
				})
				.catch(function(error) {
					console.log(error);
				});
		},
		linkCustomID() {
			axios
				.post('/api/linkCustomId', {
					id: getUID(),
					session: getSessionTicket(),
				})
				.then((response) => {
					console.log(response);
					//todo, where should the user go at this point?
				})
				.catch(function(error) {
					console.log(error);
				});
		},
		/*submit() {
      axios
        .post(`https://8EA26.playfabapi.com/Client/RegisterPlayFabUser`, {
          TitleId: "8EA26",
          Email: this.email,
          Password: this.password,
          RequireBothUsernameAndEmail: false
        })
        .then(response => {
          setSessionTicket(response.data.data.SessionTicket);
          console.log(response);
          this.linkCustomID(response);
        })
        .catch(e => {
          console.log(e);
        });
    },
    linkCustomID(response) {
      axios
        .post(
          `https://8EA26.playfabapi.com/Client/LinkCustomID`,
          {
            TitleId: "8EA26",
            CustomId: getUID(),
            ForceLink: true
          },
          {
            headers: {
              "X-authentication": getSessionTicket()
            }
          }
        )
        .then(response => {
          console.log(response);
        })
        .catch(e => {
          this.message = e.errorMessage;
        });
    }*/
	},
};
</script>
