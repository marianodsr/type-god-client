<script>
    import { goto} from '@roxi/routify'


</script>


<div class="container">
    <div class="title-container">
        <h2>Welcome to</h2>
        <img src="images/type-god-logo.svg" alt="type-god-logo">
    </div>
    <p>Type god is a game where you race against other people to see who can complete a paragraph first</p>
    <button on:click={() => $goto("../play")}>
        JOIN A GAME
    </button>
</div>


<style>
    .container {
        width: 100vw;
        height: 100vh;
        background-color: var(--purple);
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;        
        color: white;
    }
    
    .title-container {
        display: flex;
        align-items: center;
        margin-bottom: .5rem;

    }
    h1,h2,h3,h4 {
        padding: 0;
        margin: 0;
    }

    h2 {
        color: white;
        font-size: 3rem;
        margin-right: 1rem;
    }

    button {
        margin-top: 3rem;
        background-color: var(--linen);
        color: var(--purple);
        font-weight: bold;
        font-size: 2rem;
        border-radius: 6px;
        border: 4px solid var(--purple);
        cursor: pointer;
    }
    button:hover {
        background-color: var(--purple);
        color: var(--linen);
        border: 4px solid var(--linen);
    }
    img {
        width: 10rem;
    }
    p {
        font-size: 1.3rem;
        max-width: 50%;
        text-align: center;
    }
</style>