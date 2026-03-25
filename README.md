// 캔버스 생성
const canvas = createCanvas(500, 500);
const ctx = canvas.getContext("2d");

// 이미지 레이어 순서대로 합성
const layers = [
  "https://itimg.kr/3934/WH/ST.webp",   // 배경 (맨 아래)
  "https://itimg.kr/3934/WX/WX.webp",   // 날씨
  "https://itimg.kr/3934/EVNT/OFF.webp", // 변태
  "https://itimg.kr/3934/O2/FT.webp",   // 방귀
  "https://itimg.kr/3934/PJ/BF1.webp",  // 캐릭터 (맨 위)
];

for (const layerUrl of layers) {
  try {
    const img = await loadImage(layerUrl);
    ctx.drawImage(img, 0, 0, 500, 500);
  } catch (e) {
    // 이미지 없으면 스킵
  }
}

// 결과 이미지 반환
const buffer = canvas.toBuffer("image/png");
return new Response(buffer, {
  headers: { "Content-Type": "image/png" }
});
