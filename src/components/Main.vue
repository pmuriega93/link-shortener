<template>
  <div class="container">
        <input v-model="url" type="text" placeholder="Paste a link..." class="link">
        <button type="submit" class="btn" @click="shortLink">Shorten</button>

      <div v-if="shortenLink" class="short-link">
        <input v-model="shortenLink" id="shortenLink" type="text" placeholder="Shorten link here..." class="link link-copy">
        <button class="btn-copy"  @click="copyToClipboard" >
            <img src="@/assets/copy-regular.svg" alt="copy icon">
            <span class="tooltip">Copy to Clipboard</span>
        </button>
      </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            url: '',
            shortenLink: '',
            error: null
        }
    },

    methods: {
        async shortLink() {
            const { id } = await fetch('https://api-ssl.bitly.com/v4/shorten', {
                method: 'POST',
                headers: {
                    'Authorization': `${process.env.VUE_APP_API_KEY}`,
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    'long_url': this.url
                })
            })
            .then(resp => resp.json())

            this.shortenLink = id
        },

        async copyToClipboard() {
            try {
                await navigator.clipboard.writeText(this.shortenLink)
                alert('Copied!')
            }
            catch {
                alert('cannot copy!');
            }
        }
    }
}
</script>

<style scoped>
    .container {
        display: flex;
        flex-direction: column;
        justify-content: start;
        align-items: center;
        height: 80vh;
    }
    .link {
        display: block;
        color: #1A1B25;
        font-size: 24px;
        border: 2px solid #822E81;
        border-radius: 5px;
        width: 50vw;
        margin-top: 20px;
        transition: .2s;
    }

    .link::placeholder {
        color: #727272;
    }
    .link:hover,
    .link:focus {
        border: 2px solid rgba(130, 46, 129, .7);
    }

    .link-copy {
        cursor: pointer;
    }

    .btn {
        margin-top: 10px;
        color: #fff;
        background-color: #822E81;
        font-size: 26px;
        padding: 10px 15px;
        border-radius: 50px;
        cursor: pointer;
        box-shadow: 0 5px 10px rgba(0, 0, 0, .75);
        transition: .2s;
    }
    .btn:hover {
        opacity: .8;
    }

    .btn-copy {
        border: none;
        width: 20%;
        outline: none;
        background: none;
        position: absolute;
        top: 50%;
        left: 85%;
        cursor: pointer;
    }

    .btn-copy:hover {
        transform: scale(1.2);
    }
    .btn-copy:hover .tooltip {
        visibility: visible;
    }

    .short-link {
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
    }

    .tooltip {
        visibility: hidden;
        width: 120px;
        background-color: black;
        color: #fff;
        text-align: center;
        position: absolute;
        border-radius: 50px;
        top: 100%;
        left: 50%;
        margin-left: -60px;
        z-index: 2;
    }

    img {
        width: 10%;
    }
</style>