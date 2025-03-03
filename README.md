let slider;
let select;
let input;
let button;
let isBouncing = false;
let offsets = [];
let directions = [];
let linkSelect;
let iframe;

function setup() {
  createCanvas(windowWidth, windowHeight);
  
  // 創建輸入框
  input = createInput();
  input.position(10, 10);
  input.size(400, 50); // 設置輸入框的寬度和高度
  
  // 創建滑桿，範圍從 28 到 50
  slider = createSlider(28, 50, 28);
  slider.position(input.x + input.width + 10, 10);
  
  // 創建下拉選單
  select = createSelect();
  select.position(slider.x + slider.width + 10, 10);
  select.option('💐');
  select.option('🌸');
  select.option('💮');
  select.option('🏵️');
  select.option('🌹');
  select.option('🥀');
  select.option('🌺');
  select.option('🌻');
  select.option('🌼');
  select.option('🌷');
  
  // 創建按鈕
  button = createButton('開始/停止跳動');
  button.position(select.x + select.width + 10, 10);
  button.mousePressed(toggleBounce);
  
  // 創建連結下拉選單
  linkSelect = createSelect();
  linkSelect.position(button.x + button.width + 10, 10);
  linkSelect.option('淡江大學');
  linkSelect.option('教育科技學系');
  linkSelect.option('筆記');
  linkSelect.changed(openLink);
  
  // 創建 iframe
  iframe = createElement('iframe');
  iframe.position(100, 100);
  iframe.size(windowWidth - 200, windowHeight - 200);
  iframe.style('border', 'none');
}

function draw() {
  background(220);
  let inputText = input.value() + select.value();
  textAlign(CENTER, CENTER);
  
  // 根據滑桿的值動態調整文字大小
  let textSizeValue = slider.value();
  textSize(textSizeValue);
  
  textWrap(WORD);
  
  // 設置邊距
  let margin = 20;
  
  // 使用雙重迴圈來充滿整個畫面
  let index = 0;
  for (let y = textSizeValue / 2 + margin; y < height - margin; y += textSizeValue) {
    for (let x = textSizeValue / 2 + margin; x < width - margin; x += textWidth(inputText) + margin) {
      if (offsets.length <= index) {
        offsets.push(0);
        directions.push(1);
      }
      text(inputText, x, y + offsets[index]);
      index++;
    }
  }
  
  // 如果跳動狀態為真，則改變偏移量
  if (isBouncing) {
    for (let i = 0; i < offsets.length; i++) {
      offsets[i] += directions[i];
      if (offsets[i] > textSizeValue / 2 || offsets[i] < -textSizeValue / 2) {
        directions[i] *= -1;
      }
    }
  }
}

function toggleBounce() {
  isBouncing = !isBouncing;
}

function openLink() {
  let selected = linkSelect.value();
  let currentSrc = iframe.attribute('src');
  
  if (selected === '淡江大學') {
    if (currentSrc === 'https://www.tku.edu.tw/') {
      iframe.attribute('src', '');
    } else {
      iframe.attribute('src', 'https://www.tku.edu.tw/');
    }
  } else if (selected === '教育科技學系') {
    if (currentSrc === 'https://www.et.tku.edu.tw/') {
      iframe.attribute('src', '');
    } else {
      iframe.attribute('src', 'https://www.et.tku.edu.tw/');
    }
  } else if (selected === '筆記') {
    if (currentSrc === 'https://hackmd.io/@jPkCWNa-Qb2F4R0G9w5A2w/SyIofKzo1e') {
      iframe.attribute('src', '');
    } else {
      iframe.attribute('src', 'https://hackmd.io/@jPkCWNa-Qb2F4R0G9w5A2w/SyIofKzo1e');
    }
  }
}
