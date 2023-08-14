<script>
  import "./app.css";
  import { onMount } from "svelte";
  let limit = 100;
  let submissions;
  let container;

  onMount(async () => {
    try {
      container.innerText = "Loading...";
      const res = await fetch(
        `https://www.reddit.com/r/memes/top/.json?limit=${limit}&t=month`
      );
      submissions = await res.json();
      submissions = submissions.data.children;
      container.innerText = "";
    } catch (error) {
      container.innerText = "Something went wrong. Check your connection.";
    }
  });
</script>

<main>
  <h1>r/memes browser</h1>
  <div class="container" bind:this={container}>
    {#if submissions}
      {#each submissions as submission}
        {#if !submission.data.url.includes("r/")}
          <div class="submission">
            <div class="metadata">
              <div class="post-info">
                <span class="subreddit">r/{submission.data.subreddit}</span>
                <span class="author">u/{submission.data.author}</span>
              </div>
              <div class="reaction-info">
                <span class="upvotes">{submission.data.ups} </span>
                <span class="comments">{submission.data.num_comments} </span>
              </div>
            </div>
            <div>
              <h3 class="submission-title">{submission.data.title}</h3>
              <div class="content">
                <img
                  src={submission.data.url}
                  alt={submission.data.title}
                  loading="lazy"
                />
              </div>
            </div>
          </div>
        {/if}
      {/each}
    {/if}
  </div>
</main>

<!-- <style lang="less">
  @width: 300px;
  h1 {
  background-color: rgb(173, 173, 173);
  width: @width;
  }
  
</style> -->
