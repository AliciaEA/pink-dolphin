<script>
    const currentYear = new Date().getFullYear();
</script>

<footer>
    <div class="footer-grid">
        <div class="col brand-col">
            <div class="logo-area">
                <img
                    class="icon"
                    src="./images/logo.png"
                    alt="Logo Pink Dolphin"
                />
                <span class="title">Pink Dolphin</span>
            </div>
            <p>Made with love by a Hackcluber to The Zoo YSWS</p>
            <div class="status-badge">
                <span class="dot"></span> Status: Right in your heart
            </div>
        </div>

        <div class="col links-col">
            <h4>Website Pages</h4>
            <ul>
                <li><a href="/">Home</a></li>
                <li><a href="./meetup">Interactive Page 'Meetup'</a></li>
            </ul>
        </div>
        <div class="col legal-col">
            <h4>Hackclub</h4>
            <p>The Zoo YSWS</p>
        </div>
    </div>
    <div class="footer-bottom">
        <span class="copyright">&copy; {currentYear} Pink Dolphin</span>
        <span class="credits"
            >Most of the images are from Pinterest. Credits to them!</span
        >
    </div>
</footer>

<style>
    footer {
        background-color: rgb(85, 33, 55);
        padding: 6rem 2rem 1rem 2rem;
        margin-top: auto; /* Empuja el footer al fondo */
        border-top: 4px solid rgb(194, 149, 168);
        color: rgb(163, 107, 124);
    }
    .footer-grid {
        margin: 0 auto;
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 3rem;
        margin-bottom: 3rem;
    }

    .brand-col .logo-area {
        display: flex;
        align-items: center;
        gap: 0.5rem;
        margin-bottom: 1rem;
        color: rgb(134, 70, 102);
        
    }
    .logo-area .icon {
        max-width: 100px;
    }
    .title {
        font-weight: bold;
        font-size: 1.2rem;
        letter-spacing: 1px;
        background-color: rgb(189, 133, 142);
        border-radius: 30px;
        padding: 10px;
    }

    .title:hover{
        background-color: rgb(160, 94, 108);
        color: rgb(243, 164, 177);
    }
    .brand-col p {
        font-size: 1.2rem;
        line-height: 1.6;
        margin-bottom: 1.5rem;
        max-width: 300px;
    }
    .status-badge {
        display: inline-flex;
        align-items: center;
        gap: 8px;
        background: rgba(255, 190, 204, 0.1);
        color: #d38b9d;
        padding: 10px 20px;
        border-radius: 4px;
        font-weight: bold;
        font-size: 1rem;
        border: 1px solid rgba(240, 151, 163, 0.2);
    }
    .dot {
        width: 9px;
        height: 9px;
        background-color: #fc4766;
        border-radius: 50%;
        box-shadow: 0 0 5px #f30216;
        animation: pulse 2s infinite;
    }
    @keyframes pulse {
        0%,
        100% {
            opacity: 1;
            transform: scale(1);
        }
        40% {
            opacity: 0.5;
            transform: scale(1.2);
        }
    }

    footer h4 {
        margin-bottom: 1.2rem;
        letter-spacing: 1px;
        text-transform: uppercase;
        font-family: "GFS Neohellenic", sans-serif;
        font-size: 1.2rem;
        color: rgb(37, 4, 7);
    }
    ul {
        list-style: none;
        padding: 0;
    }

    li {
        margin-bottom: 0.8rem;
    }
    a {
        color: rgb(133, 94, 100);
        text-decoration: none;
        transition: color 0.2s;
        font-size: 1.2rem;
    }
    a:hover {
        color: rgb(145, 62, 83);
        text-decoration: underline;
    }
    .footer-bottom {
        border-top: 2px solid rgba(255, 255, 255, 0.1);
        padding-top: 1.5rem;
        display: flex;
        justify-content: space-between;
        font-family: var(--mono-font);
        font-size: 1rem;
        opacity: 0.6;
       
        margin: 0 auto;
    }

    @media (max-width: 600px) {
        .footer-bottom {
            flex-direction: column;
            gap: 10px;
            text-align: center;
        }
        .footer-grid {
            text-align: center;
        }
        .brand-col .logo-area,
        .status-badge {
            justify-content: center;
        }
        .brand-col p {
            margin: 0 auto 1.5rem auto;
        }
    }
</style>
