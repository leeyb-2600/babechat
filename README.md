export default {
  async fetch(request) {
    const url = new URL(request.url);
    const data = url.searchParams.get("data");
    const params = data.split("|");

    const layers = [
      "https://itimg.kr/3934/WH/ST.webp",    // 배경
      "https://itimg.kr/3934/WX/WX.webp",    // 날씨
      "https://itimg.kr/3934/EVNT/OFF.webp",  // 변태
      "https://itimg.kr/3934/O2/FT.webp",    // 방귀
      "https://itimg.kr/3934/PJ/BF1.webp",   // 캐릭터
    ];

    // SVG로 이미지 겹치기
    const svg = `
    <svg xmlns="http://www.w3.org/2000/svg" width="500" height="500">
      ${layers.map(src => `<image href="${src}" width="500" height="500"/>`).join("")}
    </svg>`;

    return new Response(svg, {
      headers: { "Content-Type": "image/svg+xml" }
    });
  }
}
