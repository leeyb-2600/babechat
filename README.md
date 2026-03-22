export default {
  async fetch(request) {
    const url = new URL(request.url);
    let data = url.searchParams.get('data') || '';
    if (!data) {
      return new Response('No data provided. 예: ?data=|[캐릭터ID]|×|[이름]|[대사]|[용변수치]|[장소]|[시간]|[돈]', { status: 400 });
    }

    data = data.trim();
    const parts = data.split('|').map(p => p.trim());

    const charId   = parts[1] || 'UNKNOWN';
    const separator = parts[2] || '×';
    const name      = parts[3] || '[이름]';
    const dialogue  = parts[4] || '[대사]';
    const poopInfo  = parts[5] || '0/1000';
    const location  = parts[6] || '[장소]';
    const time      = parts[7] || '12:00';
    const money     = parts[8] || '12,500';

    const pink = '#E9B2BD';
    const white = 'white';
    const dark = '#333';
    const gray = '#666';
    const accent = '#D53F8C'; // 용변수치 강조용 핑크-퍼플

    const svg = `
<svg width="1024" height="1024" viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg">
  <!-- 배경 이미지 자리 -->
  <image href="" width="1024" height="1024" />

  <!-- 시간 -->
  <rect x="830" y="40" width="150" height="50" rx="10" fill="${pink}" />
  <text x="905" y="73" text-anchor="middle" font-size="22" fill="${white}" font-weight="bold" font-family="sans-serif">${time}</text>

  <!-- 돈 표시 -->
  <rect x="40" y="40" width="240" height="55" rx="27" fill="${white}" stroke="${pink}" stroke-width="3" />
  <text x="160" y="78" text-anchor="middle" font-size="24" fill="${dark}" font-weight="bold" font-family="sans-serif">💰 ${money}원</text>

  <!-- 메인 패널 -->
  <rect x="50" y="720" width="924" height="230" rx="40" fill="${white}" opacity="0.9" />
  <rect x="50" y="720" width="924" height="230" rx="40" fill="${pink}" opacity="0.1" />

  <!-- 대사 라벨 -->
  <rect x="150" y="685" width="180" height="55" rx="12" fill="${pink}" />
  <text x="240" y="722" text-anchor="middle" font-size="26" fill="${white}" font-weight="bold" font-family="sans-serif">대사</text>

  <!-- 이름 -->
  <text x="380" y="722" font-size="28" fill="${dark}" font-weight="bold" font-family="sans-serif">${name}</text>

  <!-- 장소 -->
  <circle cx="95" cy="710" r="50" fill="${white}" />
  <circle cx="95" cy="710" r="45" fill="${pink}" />
  <text x="95" y="718" text-anchor="middle" font-size="32" fill="${white}" font-weight="bold" font-family="sans-serif">📍</text>
  <text x="160" y="765" font-size="22" fill="${gray}" font-family="sans-serif">${location}</text>

  <!-- 대사 본문 (y 위치 조정, 긴 텍스트 대비) -->
  <text x="100" y="830" font-size="28" fill="${dark}" font-family="sans-serif" font-weight="500">
    ${dialogue}
  </text>

  <!-- 용변수치 (이모지 공백 추가, 색상 강조) -->
  <text x="100" y="900" font-size="34" fill="${accent}" font-weight="bold" font-family="sans-serif">
    💩 용변수치: ${poopInfo}
  </text>

  <!-- × 장식 -->
  <text x="512" y="512" font-size="120" fill="${pink}" opacity="0.25" text-anchor="middle" font-family="sans-serif">${separator}</text>
</svg>
    `.trim();

    return new Response(svg, {
      headers: { 'Content-Type': 'image/svg+xml' }
    });
  }
};
