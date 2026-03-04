<template>
	<section class="container py-5" id="contact">
    <div class="text-center mb-4">
        <h2 class="fw-bold">Contact Me!</h2>
        <h4 class="text-muted">Let’s work on something together</h4>
    </div>

    <div class="row justify-content-center">
        <div class="col-12 col-md-6">

            <form>
                <div class="mb-3">
                    <label class="form-label fw-semibold">Your Name</label>
                    <input name="name" type="text" class="form-control" placeholder="Enter name">
                </div>

                <div class="mb-3">
                    <label class="form-label fw-semibold">Your Email</label>
                    <input name="email" type="email" class="form-control" placeholder="Enter email">
                </div>

                <div class="mb-3">
                    <label class="form-label fw-semibold">Message</label>
                    <textarea name="message" class="form-control" rows="4" placeholder="Write message"></textarea>
                </div>

                <button class="btn btn-dark w-100 py-2 rounded-pill">Send Message</button>
            </form>

        </div>
    </div>
</section>
</template>

<script setup>
	
import {ref, onMounted, onBeforeUnmount} from "vue";

    import {Notyf} from "notyf";
    import "notyf/notyf.min.css";

    const notyf = new Notyf();

    const WEB3FORMS_ACCESS_KEY = "4e0dba34-036a-4c1a-847d-2a3245797145";

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
            return;
        }

        isLoading.value = true;


        try{
            const response = await fetch("https://api.web3forms.com/submit", {
                method: "POST",
                headers: {
                    "Content-Type" : "application/json",
                    Accept: "application/json"
                },
                body: JSON.stringify({
                    access_key: WEB3FORMS_ACCESS_KEY,
                    subject: subject,
                    name: name.value,
                    email: email.value,
                    message: message.value
                })
            })

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
    


    const SITE_KEY = '6LcrrH8sAAAAABSz1YjsGTVe42El_Jg92o3okDdg';

    const recaptchaContainer = ref(null);
    const recaptchaWidgetId = ref(null);
    const recaptchaToken = ref('');

    // Callback called by reCAPTCHA when successful
    function onRecaptchaSuccess(token) {
      recaptchaToken.value = token;
    }

    // Callback when expired
    function onRecaptchaExpired() {
      recaptchaToken.value = '';
    }

    // Function to render the reCAPTCHA widget
    function renderRecaptcha() {
      if (!window.grecaptcha) {
        console.error('reCAPTCHA not loaded');
        return;
      }

      recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
        sitekey: SITE_KEY,
        size: 'normal', // or 'compact'
        callback: onRecaptchaSuccess,
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

    


</script>
