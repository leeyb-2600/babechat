import { createCanvas, loadImage } from '@napi-rs/canvas';

export default {
  async fetch(request) {
    const url = new URL(request.url);
    const data = url.searchParams.get("data");
    const params = data.split("|");

    const imageBase  = params[0];
    const ext        = params[1];
    const brightness = params[2].replace("b/", "");
    const backNum    = params[3].replace("back/", "");
    const name       = params[4];
    const dialogue   = params[5];
    const fontSize   = parseInt(params[6]);
    const place      = params[7];
    const time       = params[8];

    const canvas = createCanvas(500, 500);
    const ctx = canvas.getContext("2d");

    const layers = [
      `${imageBase}back/${backNum}${ext}`,
      `${imageBase}weather${ext}`,
      `${imageBase}event${ext}`,
      `${imageBase}fart${ext}`,
      `${imageBase}character${ext}`,
    ];

    for (const layerUrl of layers) {
      try {
        const img = await loadImage(layerUrl);
        ctx.drawImage(img, 0, 0, 500, 500);
      } catch (e) {}
    }

    ctx.font = `bold ${fontSize}px sans-serif`;
    ctx.fillStyle = "white";
    ctx.fillText(name, 20, 450);

    ctx.font = `${fontSize}px sans-serif`;
    ctx.fillStyle = "white";
    ctx.fillText(dialogue, 20, 480);

    ctx.font = `18px sans-serif`;
    ctx.fillStyle = "yellow";
    ctx.fillText(`${place} | ${time}`, 20, 30);

    const buffer = canvas.toBuffer("image/png");
    return new Response(buffer, {
      headers: { "Content-Type": "image/png" }
    });
  }
}
