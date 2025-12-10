<!DOCTYPE html>
<html lang="ja">
<head>
  <link rel="stylesheet" href="css/vector.css">
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>ベクトル学習</title>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/TransformControls.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/webxr/VRButton.js"></script>
</head>
<body>

<button id="showInfo" class="show-panel-btn" style="display:none;"></button>
<button id="showControls" class="show-panel-btn" style="display:none;"></button>

<div id="learningBox">
  <div class="learning-section">
    <div class="learning-header">
      <h4>📖 学習ポイント</h4>
      <button class="toggle-learning">✖</button>
    </div>
    <div class="learning-content">
      <div class="learn-item">内積を"・" 外積を"×"で表す</div>
      <div class="learn-item"><strong>内積 :</strong> 向きの近さ</div>

      <div class="learn-item"><strong>|〇·△|:</strong> 内積の絶対値</div>
      <div class="learn-item"><strong>外積 :</strong> 2ベクトルが作る平行四辺形の面積</div>
      <div class="learn-item"><strong>|〇|:</strong> ベクトルの大きさ(長さ)</div>
      <div class="learn-item" style="margin-top: 8px;">
        <span class="detail-toggle-link" id="toggleDetailBtn"> もっと詳しく ≫</span>
      </div>
    </div>
    <div class="detail-section" id="detailSection">
      <div class="detail-content">
        <h5> 内積(ドット積)</h5>
        <p><strong>定義:</strong> A·B = |A||B|cosθ</p>
        <p><strong>座標計算:</strong> A·B = AxBx + AyBy + AzBz</p>
        <p><strong>意味:</strong></p>
        <ul>
          <li>2つのベクトルの向きの近さ</li>
          <li>同じ向き(鋭角) → 正 </li>
          <li>垂直(直交) → 0 </li>
          <li>逆向き(鈍角) → 負</li>
          <li>平行(同じ向き) → Θ＝0°</li>
          <li>平行(逆向き) → Θ＝180°</li>
        </ul>
        <p><strong>性質:</strong></p>
         <ul>
          <li>a・a≧0</li>
          <li>a・a＝0となるのはa＝0のときに限る</li>
          <li>a・b=b・a</li>
        </ul>
       
        
        <h5> 外積(クロス積)</h5>
        <p><strong>定義:</strong> |A×B| = |A||B|sinθ</p>
        <p><strong>座標計算:</strong></p>
        <p>A×B = (AyBz-AzBy, AzBx-AxBz, AxBy-AyBx)</p>
        <p><strong>意味:</strong></p>
        <ul>
          <li>2つのベクトルが作る平行四辺形の面積</li>
          <li>2つのベクトルに垂直なベクトル</li>
          <li>平行(逆平行) → 0 </li>
          <li>垂直(直交) → 最大(90°)</li>
          <p></p>
          <p>右手の法則を参考に</p>
          <p>A(人差し指),B(中指),外積(親指)　</p> 
        </ul>

        <h5> 単位ベクトル</h5>
        <p><strong>意味:</strong></p>
        <ul>
          <li>長さが１のベクトル</li>
          <p>e₁＝(1,0,0)</p>
          <p>e₂＝(0,1,0)</p>
          <p>e₃＝(0,0,1)</p>
          
        </ul>

        <h5>まとめ,point</h5> 
        <ul>
          <li> 内積は角度が小さいほど大きくなる</li>
          <li> 外積は長さが長いほど大きくなる</li>
        </ul>
  
        
        <h5> 角度とベクトルの大きさ</h5>
        <p><strong>ベクトルの大きさ:</strong> |A| = √(Ax² + Ay² + Az²)</p>
        <p><strong>角度の計算:</strong> cosθ = (A·B) / (|A||B|)</p>
        <p><strong>関係式:</strong></p>
        <ul>
          <li>内積と外積: (A·B)² + |A×B|² = |A|²|B|²</li>
          <li>平行: A×B = 0</li>
          <li>垂直: A·B = 0</li>
        </ul>
      </div>
    </div>
  </div>
</div>

<div id="info">
  <div class="box-header">
    <h3>🧮 3Dベクトル</h3>
   <button class="toggle-box">✖</button>
  </div>
  <hr style="border-color: #444; margin: 10px 0;">
   <div class="calc-row">
    <span class="calc-label">A·B (内積):</span>
    <span class="calc-value" id="dotProductAB">0.00</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">A·C (内積):</span>
    <span class="calc-value" id="dotProductAC">0.00</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">B·C (内積):</span>
    <span class="calc-value" id="dotProductBC">0.00</span>
  </div>
  <hr style="border-color: #444; margin: 10px 0;">
  <div class="calc-row">
    <span class="calc-label">|A·B|:</span>
    <span class="calc-value" id="dotMagnitudeAB">0.00</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">|A·C|:</span>
    <span class="calc-value" id="dotMagnitudeAC">0.00</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">|B·C|:</span>
    <span class="calc-value" id="dotMagnitudeBC">0.00</span>
  </div>
  <hr style="border-color: #444; margin: 10px 0;">
   <div class="calc-row">
  <span class="calc-label">A×B (外積):</span>
  <span class="calc-value" id="crossVectorAB">0.00</span>
</div>
<div class="calc-row">
  <span class="calc-label">A×C (外積):</span>
  <span class="calc-value" id="crossVectorAC">0.00</span>
</div>
<div class="calc-row">
  <span class="calc-label">B×C (外積):</span>
  <span class="calc-value" id="crossVectorBC">0.00</span>
</div>
  <hr style="border-color: #444; margin: 10px 0;">
  <div class="calc-row">
    <span class="calc-label">|A×B|:</span>
    <span class="calc-value" id="crossProductAB">0.00</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">|A×C|:</span>
    <span class="calc-value" id="crossProductAC">0.00</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">|B×C|:</span>
    <span class="calc-value" id="crossProductBC">0.00</span>
  </div>
  <hr style="border-color: #444; margin: 10px 0;">
  <div class="calc-row">
    <span class="calc-label">|A|:</span>
    <span class="calc-value" id="magnitudeA">0.00</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">|B|:</span>
    <span class="calc-value" id="magnitudeB">0.00</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">|C|:</span>
    <span class="calc-value" id="magnitudeC">0.00</span>
  </div>
  <hr style="border-color: #444; margin: 10px 0;">
  <div class="calc-row">
    <span class="calc-label">∠(A,B):</span>
    <span class="calc-value" id="angleAB">0.0°</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">∠(A,C):</span>
    <span class="calc-value" id="angleAC">0.0°</span>
  </div>
  <div class="calc-row">
    <span class="calc-label">∠(B,C):</span>
    <span class="calc-value" id="angleBC">0.0°</span>
  </div>
  <div class="vr-info" id="vrInfo" style="display:none;">
      🥽 VRモード起動中
  </div>
</div>

<div id="dotProductGraph">
  <div class="graph-resize-handle"></div>
  <canvas id="graphCanvas" width="200" height="150"></canvas>
</div>

<div id="unitCircleGraph">
  <div class="graph-resize-handle"></div>
  <canvas id="unitCircleCanvas" width="180" height="180"></canvas>
</div>

<div id="controls">
  <div class="resize-handle"></div>
  <div class="box-header">
     <h3>操作パネル</h3>
    <button class="toggle-box">✖</button>
  </div>
  <h4>
    <input type="checkbox" id="toggleVectorA" checked style="margin-right: 8px; cursor: pointer; width: 18px; height: 18px; vertical-align: middle;">
    ベクトル A (青)
  </h4>
  <div class="slider-container">
    <label>X: <input type="number" class="value-input" id="vectorAX-value" value="3.0" min="-10" max="10" step="0.1"></label>
    <input type="range" id="vectorAX" min="-10" max="10" step="0.1" value="3" />
  </div>
  <div class="slider-container">
    <label>Y: <input type="number" class="value-input" id="vectorAY-value" value="2.0" min="-10" max="10" step="0.1"></label>
    <input type="range" id="vectorAY" min="-10" max="10" step="0.1" value="2" />
  </div>
  <div class="slider-container">
    <label>Z: <input type="number" class="value-input" id="vectorAZ-value" value="1.0" min="-10" max="10" step="0.1"></label>
    <input type="range" id="vectorAZ" min="-10" max="10" step="0.1" value="1" />
  </div>

  <h4>
    <input type="checkbox" id="toggleVectorB" checked style="margin-right: 8px; cursor: pointer; width: 18px; height: 18px; vertical-align: middle;">
   ベクトル B (赤)
  </h4>
  <div class="slider-container">
    <label>X: <input type="number" class="value-input red" id="vectorBX-value" value="2.0" min="-10" max="10" step="0.1"></label>
    <input type="range" id="vectorBX" min="-10" max="10" step="0.1" value="2" />
  </div>
  <div class="slider-container">
    <label>Y: <input type="number" class="value-input red" id="vectorBY-value" value="3.0" min="-10" max="10" step="0.1"></label>
    <input type="range" id="vectorBY" min="-10" max="10" step="0.1" value="3" />
  </div>
  <div class="slider-container">
    <label>Z: <input type="number" class="value-input red" id="vectorBZ-value" value="0.5" min="-10" max="10" step="0.1"></label>
    <input type="range" id="vectorBZ" min="-10" max="10" step="0.1" value="0.5" />
  </div>

  <h4>
    <input type="checkbox" id="toggleVectorC" checked style="margin-right: 8px; cursor: pointer; width: 18px; height: 18px; vertical-align: middle;">
    ベクトル C (緑)
  </h4>
  <div class="slider-container">
    <label>X: <input type="number" class="value-input green" id="vectorCX-value" value="1.0" min="-10" max="10" step="0.1"></label>
    <input type="range" id="vectorCX" min="-10" max="10" step="0.1" value="1" />
  </div>
  <div class="slider-container">
    <label>Y: <input type="number" class="value-input green" id="vectorCY-value" value="1.0" min="-10" max="10" step="0.1"></label>
    <input type="range" id="vectorCY" min="-10" max="10" step="0.1" value="1" />
  </div>
  <div class="slider-container">
    <label>Z: <input type="number" class="value-input green" id="vectorCZ-value" value="3.0" min="-10" max="10" step="0.1"></label>
    <input type="range" id="vectorCZ" min="-10" max="10" step="0.1" value="3" />
  </div>

  <button id="togglePlaneBtn">平面ON/OFF</button>
  <button id="toggleGraphBtn">グラフON/OFF</button>
  <button id="toggleCrossBtn">外積ON/OFF</button>
  <button id="toggleGridBtn">グリッド線ON/OFF</button>
  <button id="resetBtn">リセット</button>
</div>
