export default {
  async fetch(request) {
    const url = new URL(request.url);
    const data = url.searchParams.get('data') || '';

    if (!data) {
      return new Response('데이터 부족', { status: 400 });
    }

    function esc(str = '') {
      return String(str).replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;').replace(/'/g, '&apos;');
    }

    const p = data.split('|').map(v => v.trim());
    const separator = p[2] || '×';
    const name      = p[3] || '[이름]';
    const dialogue  = p[4] || '[대사]';
    const poopInfo  = p[5] || '0/1000'; // 용변수치
    const location  = p[6] || '[장소]';
    const time      = p[7] || '[시간]';
    const money     = p[8] || '0'; // 돈

    // 원본 투명 핑크 테마 색상 복원
    const pink = '#f9dadc'; 
    const white = 'white';
    const dark = '#333';

    // SVG 시작 (원본 UI 디자인 완벽 복원)
    const svg = `
<svg width="1024" height="1024" viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg">
  <rect width="1024" height="1024" fill="#fff7fa" />

  <rect x="800" y="40" width="180" height="60" rx="15" fill="${pink}" />
  <text x="890" y="78" text-anchor="middle" font-size="24" fill="white" font-weight="bold" font-family="sans-serif">${esc(time)}</text>

  <text x="50" y="80" font-size="28" fill="${dark}" font-weight="500" font-family="sans-serif">💰 소지금: ${esc(money)}원</text>

  <rect x="50" y="720" width="924" height="230" rx="40" fill="${pink}" opacity="0.3" stroke="#f0c0c0" stroke-width="2"/>
  <rect x="50" y="720" width="924" height="230" rx="40" fill="${white}" opacity="0.95" />
  
  <rect x="150" y="685" width="220" height="55" rx="15" fill="${pink}" />
  <text x="260" y="722" text-anchor="middle" font-size="26" fill="white" font-weight="bold" font-family="sans-serif">Message</text>
  
  <text x="400" y="722" font-size="28" fill="${dark}" font-weight="bold" font-family="sans-serif">${esc(name)}</text>

  <circle cx="95" cy="710" r="45" fill="${pink}" />
  <path d="M95 738 C88 730 75 720 75 708 C75 700 81 695 88 695 C92 695 94 697 95 699 C96 697 98 695 102 695 C109 695 115 700 115 708 C115 720 102 730 95 738Z" fill="white" opacity="0.9"/>

  <text x="100" y="810" font-size="28" fill="${dark}" font-family="sans-serif" font-weight="500">${esc(dialogue)}</text>

  <text x="100" y="890" font-size="36" fill="${dark}" font-weight="bold" font-family="sans-serif">💩 용변수치: ${esc(poopInfo)}</text>

  <text x="512" y="512" font-size="120" fill="${pink}" opacity="0.2" text-anchor="middle" font-family="sans-serif">${esc(separator)}</text>
</svg>
`.trim();

    return new Response(svg, {
      headers: { 'Content-Type': 'image/svg+xml; charset=utf-8' }
    });
  }
};
