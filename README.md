export default {
  async fetch(request) {
    const url = new URL(request.url);
    const data = url.searchParams.get("data");
    const params = data.split("|");

    const layers = [
      "https://itimg.kr/3934/WH/ST.webp",
      "https://itimg.kr/3934/WX/WX.webp",
      "https://itimg.kr/3934/EVNT/OFF.webp",
      "https://itimg.kr/3934/O2/FT.webp",
      "https://itimg.kr/3934/PJ/BF1.webp",
    ];

    // 이미지 fetch로 바이너리로 가져오기
    const buffers = await Promise.all(
      layers.map(async (src) => {
        const res = await fetch(src);
        return await res.arrayBuffer();
      })
    );

    // @cf/stabilityai 또는 Cloudflare Image Resizing으로 합성
    // 첫번째 이미지 베이스로 반환 (임시)
    return new Response(buffers[0], {
      headers: { "Content-Type": "image/webp" }
    });
  }
}
