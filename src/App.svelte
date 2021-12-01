<script>
  import { onMount } from "svelte";

  let users = [];
  let page = 1;
  let pages;

  $: console.log("Page is", page);

  async function fetchData() {
    const res = await fetch(`https://reqres.in/api/users?page=${page}`);
    const json = await res.json();
    users = json.data;
    page = json.page;
    pages = json.total_pages;
  }

  onMount(async () => {
    fetchData();
  });
  const setPage = (p) => {
    if (p <= pages) {
      users = [];
      setTimeout(() => {
        page = p;
        fetchData();
      }, 2000);
    }
  };
</script>

<div class="container">
  <h1>Users List</h1>
  <div class="row row-cols-1 row-cols-sm-2 row-cols-md-2 row-cols-lg-4">
    {#each users as user}
      <div class="col">
        <div class="card mb-4">
          <img src={user.avatar} class="card-img-top" alt="avatar" />
          <div class="card-body">
            <h5 class="card-title">{user.first_name}</h5>
            <h5 class="card-title">{user.last_name}</h5>
          </div>
        </div>
      </div>
    {:else}
      <!-- this block renders when users.length === 0 -->
      <div class="loading">
        <div class="spinner-border text-primary " role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
      </div>
    {/each}
  </div>

  <nav class="pagination">
    <ul>
      <li>
        <button
          type="button"
          class="btn-next-prev"
          on:click={() => setPage(page - 1)}
        >
          PREV
        </button>
      </li>

      {#each Array(pages) as number, i}
        <li>
          <button
            type="button"
            class="btn-page-number"
            on:click={() => setPage(i + 1)}
          >
            {i + 1}
          </button>
        </li>
      {/each}

      <li>
        <button
          type="button"
          class="btn-next-prev"
          on:click={() => setPage(page + 1)}
        >
          NEXT
        </button>
      </li>
    </ul>
  </nav>
</div>

<style>
  @import "https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css";
  nav > ul {
    list-style-type: none;
    display: flex;
  }
  .pagination {
    display: flex;
    justify-content: center;
  }
  .spinner-border {
    width: 3rem;
    height: 3rem;
  }
  .loading {
    display: flex;
    justify-content: center;
    margin-top: 30%;
    margin-bottom: 30%;
    width: 100%;
  }
</style>
