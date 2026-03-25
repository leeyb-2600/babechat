import { PhotonImage, watermark } from "@cf-wasm/photon";

export default {
  async fetch(request) {
    const layers = [
      "https://itimg.kr/3934/WH/ST.webp",
      "https://itimg.kr/3934/WX/WX.webp",
      "https://itimg.kr/3934/EVNT/OFF.webp",
      "https://itimg.kr/3934/O2/FT.webp",
      "https://itimg.kr/3934/PJ/BF1.webp",
    ];

    const fetchImage = async (url) => {
      const res = await fetch(url);
      const buffer = new Uint8Array(await res.arrayBuffer());
      return PhotonImage.new_from_byteslice(buffer);
    };

    let base = await fetchImage(layers[0]);

    for (let i = 1; i < layers.length; i++) {
      const layer = await fetchImage(layers[i]);
      watermark(base, layer, BigInt(0), BigInt(0));
    }

    const output = base.get_bytes_webp();

    return new Response(output, {
      headers: { "Content-Type": "image/webp" }
    });
  }
};
