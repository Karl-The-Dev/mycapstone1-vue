<script setup>

    import {ref, onMounted, onBeforeUnmount} from "vue";

	import {Notyf} from "notyf";
	import "notyf/notyf.min.css";

	const notyf = new Notyf();

	const WEB3FORMS_ACCESS_KEY = "70c7f30a-ca7d-44f4-bd48-422768b724b2";

	// email subject
	const subject = "New message from Portfolio Contact form";

	const name = ref("");
	const email = ref("");
	const message = ref("");

	const isLoading = ref(false);

	const submitForm = async()=> {

		// Check if reCAPTCHA token is present, return an error when not verified
		if(!recaptchaToken.value){
			notyf.error("Please verify that you are not a robot")
			return; // return - block the next line of codes
		}

		isLoading.value = true;


		try{
			const response = await fetch("https://api.web3forms.com/submit", {
				method: "POST",
				headers: {
					"Content-Type" : "application/json", // sends request as json format
					Accept: "application/json" // receives request as json format
				},
				// this body sends the request to api.web3forms.com/submit
				// the access_key is the key to web3forms to allow submission
				// conviert to stringify to accept the data
				body: JSON.stringify({
					access_key: WEB3FORMS_ACCESS_KEY,
					subject: subject, // subject has no value as it was delcared above
					name: name.value,
					email: email.value,
					message: message.value
				})
			})

			// get the result from the web3forms.com/submit
			const result = await response.json();

			if(result.success){
				console.log(result);
				isLoading.value = false;
				notyf.success("Message Sent!");
			}
			else{
				isLoading.value = false;
				notyf.error("Failed to send message");
			}
		}
		catch(error){
			console.log(error);
			isLoading.value = false;
			notyf.error("Failed to send message");
		}
		

	}
	
	// RECAPTCHA START --------------------------------------------------

	const SITE_KEY = '6LdTj38sAAAAAEng_olZ646hMIHxVbt49d9gHkc8';  

	const recaptchaContainer = ref(null);
	const recaptchaWidgetId = ref(null);
	const recaptchaToken = ref('');
	
	// STEP 3 - Proceed to success once captcha is processed
	// Callback called by reCAPTCHA when successful
	function onRecaptchaSuccess(token) {
		// console.log(token); // if you want to see the token 
	  recaptchaToken.value = token;
	}

	// Callback when expired
	function onRecaptchaExpired() {
	  recaptchaToken.value = '';
	}

	// Step 2 - Function to render the reCAPTCHA widget
	function renderRecaptcha() {
	  if (!window.grecaptcha) {
	    console.error('reCAPTCHA not loaded');
	    return;
	  }

	  // This generates the captcha Window in the recaptchaContainer
	  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
	    sitekey: SITE_KEY,
	    size: 'normal', // or 'compact'
	    callback: onRecaptchaSuccess, // if you put brackets onRecaptchaSuccess() - it will trigger right away. The callback waits for the action
	    'expired-callback': onRecaptchaExpired,
	  });
	}

	// Function to reset reCAPTCHA 
	function resetRecaptcha() {
	  if (recaptchaWidgetId.value !== null) {
	    window.grecaptcha.reset(recaptchaWidgetId.value);
	    recaptchaToken.value = '';
	  }
	}


	// Step 1 - Loaded webpage, it will render the recaptcha and run the renderRecaptcha
	onMounted(() => {
	  // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
	  // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
	  // Callback functions handle success and expiration events. 
	  // reCAPTCHA is reset upon form submission to clear the token.
	  const interval = setInterval(() => {
	    if (window.grecaptcha && window.grecaptcha.render) {
	      renderRecaptcha();
	      clearInterval(interval);
	    }
	  }, 100);

	  onBeforeUnmount(() => {
	    clearInterval(interval);
	  });
	});

	// RECAPTCHA END --------------------------------------------------

</script>

<template>
    <form class="container-fluid" id="contact" @submit.prevent="submitForm">
		<div class="row justify-content-center">
            <div class="col-12">
                <div class="container-fluid" >
                    <div class="row" id="contact-container">
                        <!-- Map Section - Left Side -->
                        <div class="col-md-6 p-4 p-md-5 order-md-1 order-2" id="map">
                            <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3860.395389963708!2d121.04668217599457!3d14.632485776137637!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3397b7afde2bffff%3A0xfb6ed19aaf10bfe6!2sPhilippine%20Heart%20Center!5e0!3m2!1sen!2sph!4v1699984567890!5m2!1sen!2sph" 
                                    width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade">
                            </iframe>
                            <p class="p-3 mt-3 address-text">Philippine Heart Center Bldg, East Avenue, cor Matalino St, Quezon City, Metro Manila</p>
                        </div>
                        
                       <!-- Contact Form Section - Right Side -->
                        <div class="col-md-6 p-4 p-md-5 order-md-2 order-1">
                            <h1>CONTACT</h1>				
                                                        <div class="mb-4">
                                <label for="nameInput" class="form-label">Name</label>
                                <input 
                                    type="text" 
                                    class="form-control" 
                                    id="nameInput" 
                                    placeholder="First Name M.I Last Name"
                                    v-model="name"
                                >
                            </div>
                            
                            <!-- Email Field -->
                            <div class="mb-4">
                                <label for="emailInput" class="form-label">Email address</label>
                                <input 
                                    type="email" 
                                    class="form-control" 
                                    id="emailInput" 
                                    placeholder="email@email.com"
                                    v-model="email"
                                >
                            </div>
                            
                            <!-- Message Field -->
                            <div class="mb-4">
                                <label for="messageTextarea" class="form-label">Message</label>
                                <textarea 
                                    class="form-control" 
                                    id="messageTextarea" 
                                    rows="5" 
                                    placeholder="Your message here..."
                                    v-model="message"
                                ></textarea>
                            </div>
                            
                            <!-- Images and Submit Button Section -->
                            <div class="row align-items-center mt-4">
                                <div class="col-md-8">
                                    <div class="d-flex justify-content-center justify-content-md-start py-3 social-images">
                                        <a href="https://www.linkedin.com/"><img src="/images/linkedin.png" class="img-fluid mx-2"></a>
                                        <a href="https://www.facebook.com/"><img src="/images/facebook.jpg" class="img-fluid mx-2"></a>
                                        <a href="https://www.github.com/"><img src="/images/github.png" class="img-fluid mx-2"></a>                                    
                                    </div>
                                </div>
                                <div class="col-md-4 text-md-end text-center">
                                    <button class="btn mt-3 bg-warning fw-bold" id="button-contact">Submit</button>
                                </div>

                                <!-- recaptcha checkbox -->
                                <div class="d-flex justify-content-end mt-2">
                                    <div ref="recaptchaContainer"></div>
                                </div>

                            </div>
                        </div>				
                    </div>
                </div>
            </div>
        </div>
    </form>
</template>

<style scoped>

    /* Contact */

#contact {
    background: 
	linear-gradient(
		rgba(0, 0, 0, 0.59), /*top*/
		rgba(0, 0, 0, 0.59)  /*bottom*/
	);
	background-repeat: no-repeat;
	background-size: cover;
	color: rgba(255, 255, 255, 1);
}

.social-images img {
            max-width: 50px;
            transition: transform 0.3s;
}

.social-images img:hover {
            transform: scale(1.1);
}

#button-contact {
            padding: 8px 25px;
}

#map iframe {
            width: 80%;
            height: 80%;
            min-height: 400px;
}
    
</style>
