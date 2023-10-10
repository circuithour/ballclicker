<script>
    import { onMount } from "svelte";

    onMount(() => {
        loadGame();
        setInterval(() => {
            saveGame();
        }, 20000);
    });

    setInterval(() => {
        count += rate;
    }, 1000);

    let rate = 0;
    let clickRate = 1;
    let count = 0;
    let abt = 0;
    let eabt = 0;
    let bte = 0;
    let btg = 0;
    let cbp = 0;
    let nbt = 0;
    let gp = 0;
    let showclickcount = false;
    let saveText = false;
    let ph = false;
    let tph = false;
    let ta = false;
    let mf = false;
    let mh = false;
    let ua = false;
    let gadgetDrawer = false;
    let upgradeDrawer = false;

    $: rate =
        abt * 4 +
        eabt * 8 +
        bte * 32 +
        btg * 128 +
        cbp * 128 * 2 +
        nbt * 256 * 2 +
        gp * 512 * 2;

    function saveGame() {
        saveText = true;
        const gameData = {
            clickRate,
            count,
            abt,
            eabt,
            bte,
            btg,
            cbp,
            nbt,
            gp,
            ph,
            tph,
            ta,
            mf,
            mh,
            ua,
        };
        localStorage.setItem("gameData", JSON.stringify(gameData));
        setTimeout(() => {
            saveText = false;
        }, 3000);
    }

    function loadGame() {
        const storedData = localStorage.getItem("gameData");
        if (storedData) {
            const gameData = JSON.parse(storedData);
            clickRate = gameData.clickRate;
            count = gameData.count;
            abt = gameData.abt;
            eabt = gameData.eabt;
            bte = gameData.bte;
            btg = gameData.btg;
            cbp = gameData.cbp;
            nbt = gameData.nbt;
            gp = gameData.gp;
            ph = gameData.ph;
            tph = gameData.tph;
            ta = gameData.ta;
            mf = gameData.mf;
            mh = gameData.mh;
            ua = gameData.ua; // Ultra Hand
        }
    }

    function destroyGame() {
        localStorage.removeItem("gameData");
        rate = 0;
        clickRate = 1;
        count = 0;
        abt = 0;
        eabt = 0;
        bte = 0;
        btg = 0;
        cbp = 0;
        nbt = 0;
        gp = 0;
        ph = false;
        tph = false;
        ta = false;
        mf = false;
        mh = false;
        ua = false;
    }

    let isMobileView = false;

    function checkMobileView() {
        isMobileView = window.innerWidth <= 768;
    }

    if (typeof window !== "undefined") {
        checkMobileView(); // Initial check
        window.addEventListener("resize", checkMobileView);
    }

    function toggleGDrawer() {
        gadgetDrawer = !gadgetDrawer;
        if (upgradeDrawer === true) {
            upgradeDrawer = !upgradeDrawer;
        }
    }

    function toggleUDrawer() {
        upgradeDrawer = !upgradeDrawer;
        if (gadgetDrawer === true) {
            gadgetDrawer = !gadgetDrawer;
        }
    }

    function closeDrawers() {
        upgradeDrawer = false;
        gadgetDrawer = false;
    }

    function addBall() {
        count += clickRate;
        showclickcount = true;
        setTimeout(() => {
            showclickcount = false;
        }, 500);
    }

    function buyGadget(price, gadget) {
        if (count >= price) {
            count -= price;
            gadget++;
        }
        return { gadget, count };
    }

    function upgradeHand(price, upgrade, power) {
        if (count >= price && !upgrade) {
            count -= price;
            upgrade = true;
            clickRate += power;
        }
        return { upgrade, count };
    }
</script>

<svelte:head>
    <title>Ball Clicker</title>
    <meta name="title" content="Ball Clicker" />
    <meta name="description" content="Click the balls as fast as you can" />
    <meta name="keywords" content="balls, clicker, idle, game" />
    <meta name="robots" content="index, nofollow" />
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="language" content="English" />
</svelte:head>

<div class="container">
    <div
        class="shop {isMobileView
            ? gadgetDrawer
                ? 'slide-in'
                : 'slide-out'
            : ''}"
    >
        <div
            class={isMobileView && gadgetDrawer ? "backdrop" : "megadie"}
            on:click={closeDrawers}
            role="button"
            tabindex=""
        />
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(500, abt);
                abt = result.gadget;
                count = result.count;
            }}
        >
            <p>Automatic Ball Toucher</p>
            <span>
                <p class={count < 500 ? "unaffordable" : "affordable"}>
                    500 Balls
                </p>
                <p>x{abt}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(1000, eabt);
                eabt = result.gadget;
                count = result.count;
            }}
        >
            <p>Enhanced Automatic Ball Toucher</p>
            <span>
                <p class={count < 1000 ? "unaffordable" : "affordable"}>
                    1000 Balls
                </p>
                <p>x{eabt}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(10000, bte);
                bte = result.gadget;
                count = result.count;
            }}
        >
            <p>Ball Touching Employee</p>
            <span>
                <p class={count < 10000 ? "unaffordable" : "affordable"}>
                    10000 Balls
                </p>
                <p>x{bte}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(100000, btg);
                btg = result.gadget;
                count = result.count;
            }}
        >
            <p>Ball Touching Goblin</p>
            <span>
                <p class={count < 100000 ? "unaffordable" : "affordable"}>
                    100000 Balls
                </p>
                <p>x{btg}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(500000, cbp);
                cbp = result.gadget;
                count = result.count;
            }}
        >
            <p>Counterfeit Ball Printer</p>
            <span>
                <p class={count < 500000 ? "unaffordable" : "affordable"}>
                    500000 Balls
                </p>
                <p>x{cbp}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(1000000, nbt);
                nbt = result.gadget;
                count = result.count;
            }}
        >
            <p>Neutron Ball Tech</p>
            <span>
                <p class={count < 1000000 ? "unaffordable" : "affordable"}>
                    1000000 Balls
                </p>
                <p>x{nbt}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(5000000, gp);
                gp = result.gadget;
                count = result.count;
            }}
        >
            <p>Gamma Particles</p>
            <span>
                <p class={count < 5000000 ? "unaffordable" : "affordable"}>
                    5000000 Balls
                </p>
                <p>x{gp}</p>
            </span>
        </div>
    </div>
    <div class="stage">
        <div class="information">
            <span class="rate info">{rate}/sec</span>
            <span class="clickRate info">{clickRate} Ball Power</span>
            <span class="count info">{count}</span>
        </div>
        <div class="counters">
            {#if rate > 0}
                <div class="ballFeed counter info">+{rate}</div>
            {/if}
            {#if showclickcount}
                <div class="ballClickFeed counter info">+{clickRate}</div>
            {/if}
        </div>
        <div id="button" role="button" tabindex="" on:click={addBall} />
        <div class="footer">
            <button on:click={toggleGDrawer} id="gadgetbutton">Gadgets</button>
            <button on:click={saveGame}>Save</button>
            {#if saveText}
                <p id="savetext">Game has been saved.</p>
            {/if}
            <button on:click={destroyGame}>Reset</button>
            <button on:click={toggleUDrawer} id="upgradebutton">Upgrades</button
            >
        </div>
    </div>
    <div
        class="shop2 {isMobileView
            ? upgradeDrawer
                ? 'slide-in-r'
                : 'slide-out-r'
            : ''}"
    >
        <div
            class={isMobileView && upgradeDrawer ? "backdropalt" : "megadie"}
            on:click={closeDrawers}
            role="button"
            tabindex=""
        />
        <div
            role="button"
            tabindex=""
            on:click={() => {
                const result = upgradeHand(100, ph, 4);
                ph = result.upgrade;
                count = result.count;
            }}
            class={ph ? "die" : "shopitemalt"}
        >
            <span>
                <p class={count < 100 && !ph ? "unaffordable" : "affordable"}>
                    100 Balls
                </p>
                {#if ph}
                    <p class="affordable">✓</p>
                {:else}
                    <p class="unaffordable">✗</p>
                {/if}
            </span>
            <p>Plastic Hand</p>
        </div>
        <div
            role="button"
            tabindex=""
            on:click={() => {
                const result = upgradeHand(1000, tph, 16);
                tph = result.upgrade;
                count = result.count;
            }}
            class={tph ? "die" : "shopitemalt"}
        >
            <span>
                <p class={count < 1000 && !tph ? "unaffordable" : "affordable"}>
                    1000 Balls
                </p>
                {#if tph}
                    <p class="affordable">✓</p>
                {:else}
                    <p class="unaffordable">✗</p>
                {/if}
            </span>
            <p>Two Plastic Hands</p>
        </div>
        <div
            role="button"
            tabindex=""
            on:click={() => {
                const result = upgradeHand(5000, ta, 24);
                ta = result.upgrade;
                count = result.count;
            }}
            class={ta ? "die" : "shopitemalt"}
        >
            <span>
                <p class={count < 5000 && !ta ? "unaffordable" : "affordable"}>
                    5000 Balls
                </p>
                {#if ta}
                    <p class="affordable">✓</p>
                {:else}
                    <p class="unaffordable">✗</p>
                {/if}
            </span>
            <p>Third Arm</p>
        </div>
        <div
            role="button"
            tabindex=""
            on:click={() => {
                const result = upgradeHand(100000, mf, 128);
                mf = result.upgrade;
                count = result.count;
            }}
            class={mf ? "die" : "shopitemalt"}
        >
            <span>
                <p
                    class={count < 100000 && !mf
                        ? "unaffordable"
                        : "affordable"}
                >
                    100000 Balls
                </p>
                {#if mf}
                    <p class="affordable">✓</p>
                {:else}
                    <p class="unaffordable">✗</p>
                {/if}
            </span>
            <p>Magic Fingers</p>
        </div>
        <div
            role="button"
            tabindex=""
            on:click={() => {
                const result = upgradeHand(500000, mh, 1024);
                mh = result.upgrade;
                count = result.count;
            }}
            class={mh ? "die" : "shopitemalt"}
        >
            <span>
                <p
                    class={count < 500000 && !mh
                        ? "unaffordable"
                        : "affordable"}
                >
                    500000 Balls
                </p>
                {#if mh}
                    <p class="affordable">✓</p>
                {:else}
                    <p class="unaffordable">✗</p>
                {/if}
            </span>
            <p>Magic Hands</p>
        </div>
        <div
            role="button"
            tabindex=""
            on:click={() => {
                const result = upgradeHand(2000000, ua, 5016);
                ua = result.upgrade;
                count = result.count;
            }}
            class={ua ? "die" : "shopitemalt"}
        >
            <span>
                <p
                    class={count < 500000 && !ua
                        ? "unaffordable"
                        : "affordable"}
                >
                    2000000 Balls
                </p>
                {#if ua}
                    <p class="affordable">✓</p>
                {:else}
                    <p class="unaffordable">✗</p>
                {/if}
            </span>
            <p>Ultra Hand</p>
        </div>
    </div>
</div>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Archivo+Black&family=Black+Ops+One&family=Gabarito&family=Oxygen&family=Poppins&family=Prompt&family=Roboto:wght@900&display=swap");
    @import url("https://fonts.googleapis.com/css2?family=Fragment+Mono&display=swap");

    * {
        font-size: 1.2rem;
        font-family: gabarito, sand-serif;
        user-select: none;
    }

    .container {
        display: flex;
        flex-direction: row;
        flex: auto;
    }

    .shop {
        display: flex;
        flex: 25%;
        flex-direction: column;
    }

    .shopitem {
        display: flex;
        align-items: center;
        padding: 0rem 1rem;
        height: 100px;
        border-bottom: solid 1px #ffffff10;
        justify-content: space-between;
        cursor: pointer;
        transition: all 0.3s ease-in-out;
        text-align: right;
    }

    .shopitemalt {
        display: flex;
        align-items: center;
        padding: 0rem 1rem;
        height: 100px;
        border-bottom: solid 1px #ffffff10;
        justify-content: space-between;
        cursor: pointer;
        transition: all 0.3s ease-in-out;
        text-align: left;
    }

    .die {
        display: flex;
        align-items: center;
        padding: 0rem 1rem;
        height: 100px;
        border-bottom: solid 1px #ffffff10;
        justify-content: space-between;
        cursor: not-allowed;
        color: #a1a1a1;
        transition: all 0.3s ease-in-out;
        text-align: left;
    }

    .megadie {
        display: none;
    }

    .shopitem:hover,
    .shopitemalt:hover {
        background-color: #ffffff10;
        transition: all 0.3s ease-in-out;
    }

    .unaffordable {
        color: rgb(255, 137, 137);
        transition: all 0.2s ease-in-out;
    }

    .affordable {
        color: rgb(184, 255, 182);
        transition: all 0.2s ease-in-out;
    }

    .stage {
        position: relative;
        display: flex;
        flex-direction: column;
        flex: 50%;
        border-left: solid 1px #ffffff10;
        border-right: solid 1px #ffffff10;
        justify-content: center;
        align-items: center;
    }

    .information {
        display: flex;
        top: 0;
        width: 100%;
        position: absolute;
        height: 100px;
        background-color: #ffffff10;
        justify-content: space-evenly;
        align-items: center;
    }

    .info {
        font-family: "Fragment Mono", monospace;
    }

    .counters {
        position: absolute;
        width: 200px;
        height: 30px;
        top: 20%;
        display: flex;
        justify-content: space-evenly;
    }

    .counters .counter {
        position: absolute;
        width: 200px;
        top: 20%;
    }

    #button {
        background-color: hsl(290, 61%, 29%);
        border-radius: 50%;
        width: 200px;
        height: 200px;
        cursor: pointer;
        transition: background-color 0.1s ease-in-out;
    }

    #button:hover {
        background-color: hsl(290, 61%, 39%);
        transition: background-color 0.1s ease-in-out;
    }

    #button:active {
        transform: translateY(4px);
        transition: transform 0.1s ease-in-out;
    }

    .footer {
        display: flex;
        bottom: 0;
        width: 100%;
        position: absolute;
        height: 100px;
        background-color: #ffffff10;
        justify-content: space-evenly;
        align-items: center;
    }

    #savetext {
        margin: 0;
        padding: 0;
        position: fixed;
        animation: fade 3s ease-out;
    }

    @keyframes fade {
        0% {
            opacity: 100%;
        }
        33% {
            opacity: 100%;
        }
        66% {
            opacity: 60%;
        }
        100% {
            opacity: 0%;
        }
    }

    button {
        width: 128px;
        height: 100%;
        border-radius: 5px;
        background-color: transparent;
        color: white;
        font-family: "Fragment Mono", monospace;
        border-top: none;
        border-right: solid 3px #ffffff10;
        border-left: solid 3px #ffffff10;
        border-bottom: none;
        transition: all 0.3s ease-in-out;
    }

    button:hover {
        background-color: #ffffff10;
        transition: all 0.3s ease-in-out;
    }

    .shop2 {
        display: flex;
        flex: 25%;
        flex-direction: column;
    }

    .ballFeed {
        animation: ballfeed 1s linear infinite;
    }

    .ballClickFeed {
        animation: ballfeed 0.5s linear;
    }

    @keyframes ballfeed {
        0% {
            transform: scale(1);
            opacity: 100%;
        }
        50% {
            transform: scale(1.2);
            opacity: 50%;
        }
        100% {
            transform: scale(1.5);
            opacity: 0%;
        }
    }

    #gadgetbutton {
        display: none;
    }

    #upgradebutton {
        display: none;
    }

    @media (max-width: 750px) {
        * {
            font-size: 14px;
        }

        .container {
            overflow: hidden;
            width: 100vw;
            position: relative;
        }

        .counters {
            transform: translateY(50px);
        }

        .shop {
            left: -100%;
            position: absolute;
            background-color: #000;
            z-index: 3;
            top: 116px;
            bottom: 100px;
            overflow-y: auto;
            max-height: 80vh;
        }

        .shop2 {
            right: 100%;
            position: absolute;
            background-color: #000;
            z-index: 3;
            top: 116px;
            bottom: 100px;
            overflow-y: auto;
            max-height: 80vh;
        }

        .footer {
            z-index: 4;
        }

        .information,
        .footer {
        }

        .information {
            padding-top: 1rem;
        }

        .shop,
        .stage,
        .shop2 {
            flex: 100;
            flex-direction: column;
            overflow-x: hidden;
        }

        .backdrop {
            background-color: hsla(0, 0%, 0%, 0);
            right: -299px;
            position: fixed;
            z-index: 999999;
            top: 116px;
            bottom: 100px;
            max-height: 74vh;
            width: 100%;
            overflow-x: hidden;
        }

        .backdropalt {
            background-color: hsla(0, 0%, 0%, 0);
            left: -201px;
            position: fixed;
            z-index: 999999;
            top: 116px;
            bottom: 100px;
            max-height: 74vh;
            width: 100%;
            overflow-x: hidden;
        }

        .stage {
            flex-direction: column;
        }

        #gadgetbutton {
            display: block;
        }

        #upgradebutton {
            display: block;
        }

        .slide-in {
            left: 0;
        }

        .slide-out {
            left: -100%;
        }

        .slide-in-r {
            right: 0;
        }

        .slide-out-r {
            right: -100%;
        }

        .shop.slide-in,
        .shop.slide-out,
        .shop2.slide-in-r,
        .shop2.slide-out-r {
            transition: all 0.3s cubic-bezier(0.65, 0.05, 0.36, 1);
        }

        #savetext {
            transform: translateY(-5rem);
        }

        #button,
        .shopitem,
        .shopitemalt {
            touch-action: manipulation;
        }
    }
</style>
