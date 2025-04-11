<script>
  let cvFile = null;
  let jobDescription = '';
  let adaptedCV = null;
  let errorMessage = null;

  const handleCVUpload = (event) => {
    cvFile = event.target.files[0];
  };

  const adaptCV = async () => {
    if (!cvFile || !jobDescription) {
      errorMessage = 'Please upload a CV and provide a job description.';
      return;
    }

    errorMessage = null;

    const formData = new FormData();
    formData.append('cv', cvFile);
    formData.append('job_description', jobDescription);

    try {
      const response = await fetch('5IpiGULyugntFMbvF64Cg50BPLbCkbOc', {
        method: 'POST',
        body: formData,
      });

      if (!response.ok) {
        throw new Error('Failed to adapt CV. Please try again.');
      }

      const data = await response.json();
      adaptedCV = data.adapted_cv; // Update based on API response structure
    } catch (error) {
      errorMessage = error.message;
    }
  };
</script>

<main>
  <h1>Adapt Your CV for a Job Offer</h1>

  <label>
    Upload your CV:
    <input type="file" accept=".pdf,.doc,.docx" on:change={handleCVUpload} />
  </label>

  <label>
    Job Description:
    <textarea bind:value={jobDescription} placeholder="Paste the job description here"></textarea>
  </label>

  <button on:click={adaptCV}>Adapt CV</button>

  {#if errorMessage}
    <p style="color: red;">{errorMessage}</p>
  {/if}

  {#if adaptedCV}
    <h2>Adapted CV</h2>
    <p>{adaptedCV}</p>
    <!-- Optionally provide a download link -->
    <a href={`data:text/plain;charset=utf-8,${encodeURIComponent(adaptedCV)}`} download="adapted_cv.txt">Download Adapted CV</a>
  {/if}
</main>

<style>
  main {
    padding: 1rem;
    font-family: Arial, sans-serif;
  }

  label {
    display: block;
    margin: 1rem 0;
  }

  textarea {
    display: block;
    width: 100%;
    height: 100px;
  }

  button {
    margin: 1rem 0;
    padding: 0.5rem 1rem;
    font-size: 1rem;
    cursor: pointer;
  }

  p {
    margin: 1rem 0;
  }
</style>