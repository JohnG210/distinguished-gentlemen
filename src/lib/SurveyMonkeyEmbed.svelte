<script>
  import { onMount } from 'svelte';
  export let surveyScriptUrl;

  onMount(() => {
    const embedContainer = document.querySelector('.surveymonkey-embed');
    if (!embedContainer) {
      console.error("SurveyMonkeyEmbed: Target container '.surveymonkey-embed' not found.");
      return;
    }

    // Check if the SurveyMonkey SDK script is already loaded
    const tempDiv = document.createElement('div'); // Create a temporary container for the embed

    // Check if the SurveyMonkey SDK script is already loaded
    if (!document.getElementById('smcx-sdk')) {
      const script = document.createElement('script');
      script.type = 'text/javascript';
      script.async = true;
      script.id = 'smcx-sdk';
      script.src = surveyScriptUrl;
      tempDiv.appendChild(script); // Append to tempDiv first
    } else {
      console.warn("SurveyMonkeyEmbed: 'smcx-sdk' script already loaded. Skipping duplicate script injection.");
    }

    // Always append the anchor tag
    const anchor = document.createElement('a');
    anchor.style.cssText = 'font: 12px Helvetica, sans-serif; color: #999; text-decoration: none;';
    anchor.href = 'www.surveymonkey.com';
    anchor.textContent = 'Create your own user feedback survey';
    tempDiv.appendChild(anchor); // Append to tempDiv first
    
    // Clear previous content and append the new temporary div
    embedContainer.innerHTML = '';
    embedContainer.appendChild(tempDiv);
  });
</script>

<div class="surveymonkey-embed">
  <!-- SurveyMonkey embed code will be injected here -->
</div>

<style>
  /* Add any specific styling for the embed here */
</style>