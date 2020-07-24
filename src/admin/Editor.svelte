<script>
    import { onMount } from 'svelte';
    import { saveAs } from 'file-saver';

    let canvas
    let image
    let x_range
    let y_range
    let r_range
    let text_input
    let ctx = null

    let text_index = 0


    let fontName = "Arial"
    let fontSize = 50
    let text = "Example Text "
    let textAlign = "left"
    let fontColor = "white"
    let auto_center_x = false
    let auto_center_y = false
    let x = 50
    let y = 100
    let r = 0

    let selected_color
    let selected_font
    let selected_align

    let fonts = [
        {id: 1, name: "Arial"},
        {id: 2, name: "Montserrat"},
        {id: 3, name: "Consolas"}
    ]

    let colors = [
        {id:1, name: "white"},
        {id:2, name: "black"}
    ]

    let texts = [
        {id: 1}
    ]

    let aligns = [
        {id: 1, pos: "left"},
        {id: 2, pos: "center"},
        {id: 3, pos: "right"},
    ]

    let confs = [
        {
            id: 1,
            text: text + " 1",
            textAlign: textAlign,
            fontName: fontName,
            fontSize: fontSize,
            fontColor: fontColor,
            x_center: auto_center_x,
            y_center: auto_center_y,
            x: x,
            y: y
        }
    ]

    onMount(() =>{
        // const ctx = 
        ctx = canvas.getContext('2d');
        canvas.width = image.width;
        canvas.height = image.height;
        canvas.crossOrigin = "Anonymous";
        
        text_input.value = confs[text_index].text

        ctx.drawImage(image, 0, 0)
        ctx.font = confs[text_index].fontSize + "pt " + confs[text_index].fontName
        ctx.fillStyle = confs[text_index].fontColor
        ctx.textAlign = confs[text_index].textAlign
        ctx.textBaseline="middle";
        ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)

        x_range.min = 0
        x_range.max = canvas.width
        x_range.value = x

        y_range.min = 0
        y_range.max = canvas.height
        y_range.value = y

        // r_range.min = 0
        // r_range.max = 2
        // r_range.value = 0
        
    })

    let setText = (e) => {
        confs[text_index].text = e.target.value
        ctx.clearRect(0,0, canvas.width, canvas.height)
        ctx.drawImage(image, 0, 0)
        ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)
    }

    let setFontName = (selected_font) => {
        confs[text_index].fontName = selected_font.name
        // console.log(fontName)
        ctx.clearRect(0,0, canvas.width, canvas.height)
        ctx.drawImage(image, 0, 0)
        ctx.font = confs[text_index].fontSize + "pt " + confs[text_index].fontName
        ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)
    }

    let setFontSize = (e) => {
        confs[text_index].fontSize = e.target.value
        
        if(fontSize != null && fontSize > 2){
            ctx.clearRect(0,0, canvas.width, canvas.height)
            ctx.drawImage(image, 0, 0)
            ctx.font = confs[text_index].fontSize + "pt " + confs[text_index].fontName
            ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)
        }

        
    }

    let setTextAlign = (align) => {
        confs[text_index].textAlign = align.pos
        // console.log(align)
        ctx.clearRect(0,0, canvas.width, canvas.height)
        ctx.drawImage(image, 0, 0)
        ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)
        ctx.textAlign = confs[text_index].textAlign
    }

    let setColor = (color) => {
        confs[text_index].fontColor = color.name
        // console.log(fontColor)
        ctx.clearRect(0,0, canvas.width, canvas.height)
        ctx.drawImage(image, 0, 0)
        ctx.fillStyle = confs[text_index].fontColor
        ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)
    }

    let setAutoCenterX = (ischecked) => {
        let t = ctx.measureText(confs[text_index].text)

        if(ischecked){
            ctx.clearRect(0,0, canvas.width, canvas.height)
            ctx.drawImage(image, 0, 0)
            // x = (canvas.width/2) - (t.width/2)
            ctx.textAlign = "center"
            confs[text_index].x = canvas.width/2
            
            ctx.textAlign = confs[text_index].textAlign
            ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)

        }
    }

    let setAutoCenterY = (ischecked) => {
        // let t = ctx.measureText(text)

        if(ischecked){
            ctx.clearRect(0,0, canvas.width, canvas.height)
            ctx.drawImage(image, 0, 0)
            confs[text_index].y = (canvas.height/2) + (fontSize/2)
            ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)
  
        }

    }

    let setX = (e) => {
        confs[text_index].x = e.target.value
        // ctx.clearRect(0,0, canvas.width, canvas.height)
        ctx.drawImage(image, 0, 0)
        ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)
    }

    let setY = (e) => {
        confs[text_index].y = e.target.value
        ctx.clearRect(0,0, canvas.width, canvas.height)
        ctx.drawImage(image, 0, 0)
        ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)
    }
    
    // let setRotate = (e) => {
    //     r = e.target.value
    //     ctx.clearRect(0,0, canvas.width, canvas.height)
    //     ctx.drawImage(image, 0, 0)
    //     ctx.save();
    //     ctx.textAlign="center";
    //     ctx.textBaseline="middle";
    //     // ctx.translate(x/2,y/2);
    //     ctx.rotate(Math.PI / r)
    //     ctx.fillText(text, x, y)
    //     ctx.restore()
    // }

    let plusBtnOnClick = () => {
        confs = [...confs, {
            id: confs.length + 1,
            text: text + (confs.length + 1),
            textAlign: textAlign,
            fontName: fontName,
            fontSize: fontSize,
            fontColor: fontColor,
            x_center: auto_center_x,
            y_center: auto_center_y,
            x: x,
            y: y
        }]

        text_index = confs.length - 1

        update()
        // console.log(texts)
    }

    let minBtnOnClick = () => {
        confs.pop()
        confs = confs
    }

    let textBtnOnClick = (counter) => {
        console.log(counter)
    }

    let download = () => {
        canvas.toBlob(function(blob) {
            saveAs(blob, "image.png");
        });
    }

    let update = () => {
        console.log("terpanggil")
        text_input.value = confs[text_index].text
        ctx.fillText(confs[text_index].text, confs[text_index].x, confs[text_index].y)
    }

    // console.log(selected_font)
    
</script>

<header>
    <button class="header-btn header-plus-btn" on:click={plusBtnOnClick}>+</button>
    <button class="header-btn header-min-btn" on:click={minBtnOnClick}>-</button>
    <button class="header-btn header-upload-btn">upload</button>
    <button class="header-btn header-save-btn">save</button>
    <button class="header-btn header-save-btn" on:click={download}>download</button>
    <button class="header-btn header-home-btn">home</button>
</header>
<main>
    <div class="main-left">
        <ul>
            {#each confs as conf}
                <li>
                    <button on:click={textBtnOnClick(conf.id)}>{conf.id}</button>
                </li>
            {/each}
            
            
        </ul>
    </div>
    <div class="main-mid">
        
        <img src="/img/file.jpg" alt="gambar" id="image" bind:this={image}>
        
        

        <canvas
            bind:this={canvas}
        ></canvas>
        
        
    </div>
    <div class="main-right">
        <div class="main-right-container">
        
            <label for="">T</label>
            <input type="text" bind:this={text_input} on:input={setText}>
            <div class="font">
                <div>
                    <label for="">F</label>
                    <!-- svelte-ignore a11y-no-onchange -->
                    <select class="select-font" bind:value={selected_font} on:change={setFontName(selected_font)}>
                        {#each fonts as font}
                            <option value={font}>
                                {font.name}
                            </option>
                        {/each}
                    </select>
                    
                </div>
                <input type="number" class="size-font" value={fontSize} on:input={setFontSize}>
                   
            </div>

            <label for="">A</label>
            <!-- svelte-ignore a11y-no-onchange -->
            <select bind:value={selected_align} on:change={setTextAlign(selected_align)}>
                {#each aligns as align}
                    <option value={align}>
                        {align.pos}
                    </option>
                {/each}
            </select>
            
            <label for="">C</label>
            <!-- svelte-ignore a11y-no-onchange -->
            <select bind:value={selected_color} on:change={setColor(selected_color)}>
                {#each colors as color}
                    <option value={color}>
                        {color.name}
                    </option>
                {/each}
            </select>


            <div class="checkbox">
                <span>Auto center X</span>
                <input type="checkbox" bind:checked={auto_center_x} on:change={setAutoCenterX(auto_center_x)}>
            </div>
            <div class="checkbox">
                <span>Auto center Y</span>
                <input type="checkbox" bind:checked={auto_center_y} on:change={setAutoCenterY(auto_center_y)}>
            </div>

            <div class="range">
                <span>X</span>
                <input type="range" disabled={auto_center_x} bind:this={x_range} on:input={setX}>
                <span>{parseInt(x)}</span>
            </div>

            <div class="range">
                <span>Y</span>
                <input type="range" disabled={auto_center_y} bind:this={y_range} on:input={setY}>
                <span>{parseInt(y)}</span>
            </div>

            <!-- <div class="range">
                <span>R</span>
                <input type="range" bind:this={r_range} on:input={setRotate} step={0.01}>
                <span>{r}</span>
            </div> -->
            
        </div>
    </div>
    
    
</main>

<style>

    header{
        background-color: #6A3737;
        padding: 10px 20px;
        font-family: 'Asap';
        font-weight: bold;
        color: black;
    }

    .header-btn{
        background-color: #6A3737;
        height: 30px;
        font-family: 'Montserrat';
        font-size: 12px;
        margin: auto 3px;
        border-radius: 5px;
        border: 1px solid white;
        color: white;
    }

    .header-btn:hover{
        background-color: #6A3740;
        cursor: pointer;
    }

    .header-plus-btn, .header-min-btn{
        width: 30px;
        font-weight: bolder;
    }
    
    .header-upload-btn, .header-save-btn{
        padding: 0 12px;
    }

    .header-home-btn{
        float: right;
        padding: 0 12px;
    }

    main{
        height: 645px;
        /* position: fixed; */
        display: flex;
        justify-content: start;
    }

    .main-left{
        float: left;
        background-color: #FDFDFD;
        width: 75px;
        border-right: 20px solid #c7c7c7;
        /* position: fixed; */
    }

    .main-left ul{
        margin: 0;
        padding: 0;
    }

    .main-left ul li{
        list-style-type: none;
        
    }

    .main-left ul li button{
        margin: 0;
        width: 100%;
        background-color: #FDFDFD;
        border: 0;
    }

    .main-left ul li button:hover{
        cursor: pointer;
        background-color: #EDEDED;
    }

    .main-left ul li button:focus{
        background-color: #EDEDED;
    }

    .main-mid{
        /* height: 500px; */
        position: relative;
        width: 100%;
        background-color: #c7c7c7;
        display: flex;
        align-items: center;
        /* padding: 0 30px; */
        z-index: -1;
    }

    .main-mid canvas{
        /* height: 100%; */
        width: 100%;
        /* width: 500px; */
        position: absolute;
        /* padding: 20px; */
        margin: auto 0;
    }

    .main-mid img{
        width: 100%;
        display: none;
    }

    .main-right{
        width: 300px;
        background-color: #FDFDFD;
        padding: 20px 20px;
        border-left: 20px solid #c7c7c7;
    }

    .main-right .main-right-container{
        /* margin: 0 auto; */
        text-align: center;
        width: 100%;
        /* padding: 20px 0; */
        /* margin-bottom: 10px; */
    }

    .main-right .main-right-container label{
        /* text-align: left; */
        position: absolute;
        padding: 10px 0 0 0;
        min-width: 40px;
        text-align: center;
        font-family: 'Montserrat';
        font-size: 12px;
        font-weight: bolder;
        color: #9F9F9F;

    }

    .main-right .main-right-container input[type="text"], .main-right .main-right-container select{
        text-indent: 30px;
        border: 1px solid #6A3737;
        border-radius: 3px;
        width: 100%;
    }

    .main-right .main-right-container .font .select-font{
        width: 162px;
    }

    .main-right .main-right-container .font{
        display: flex;
        justify-content: space-between;
    }

    .main-right .main-right-container .font .size-font{
        width: 50px;
        text-indent: 0;
        border: 1px solid #6A3737;
        border-radius: 3px;
    }

    .main-right .main-right-container .checkbox{
        display: flex;
        justify-content: space-between;
        height: 40px;
        /* padding: 0 5px; */
    }

    .main-right .main-right-container .checkbox input[type="checkbox"]{
        margin: auto 0;

    }

    .main-right .main-right-container .checkbox span{
        margin: auto 0;

    }

    .main-right .main-right-container .range{
        display: flex;
        justify-content: space-between;
        padding: 10px 0;
        /* height: 40px */
    }

    .main-right .main-right-container .range span, .main-right .main-right-container .range input{
        margin: auto 0;
    }

    /* .main-right .main-right-container .range input{
        background-color: black;
    } */

    .main-right .main-right-container .range span{
        padding: 0 5px 3px 5px ;
    }

    input[type="range"] {
        -webkit-appearance: none;
        width: 100%;
        height: 5px;
        border-radius: 10px;  
        background: #C4C4C4;
        outline: none;
        /* opacity: 0.7; */
        /* -webkit-transition: .2s; */
        /* transition: opacity .2s; */
    }

    input[type="range"]::-webkit-slider-thumb {
        -webkit-appearance: none;
        appearance: none;
        width: 20px;
        height: 20px;
        border-radius: 50%; 
        background: #6A3737;
        cursor: ew-resize;
    }

    input[type="range"]::-moz-range-thumb {
        width: 25px;
        height: 25px;
        border-radius: 50%;
        background: #6A3737;
        cursor: pointer;
    }
    

</style>
