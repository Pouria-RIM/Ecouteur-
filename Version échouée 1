<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Multi-Pitch Detector (Intervals)</title>
  <style>
    body { font-family: Arial, sans-serif; text-align: center; background: #111; color: #0f0; padding: 50px; }
    button { padding: 20px 40px; font-size: 24px; background: #222; color: #0f0; border: 2px solid #0f0; cursor: pointer; }
    #status { font-size: 26px; margin: 30px; min-height: 120px; }
    .freq { margin: 8px; font-size: 22px; }
  </style>
</head>
<body>
  <h1>Multi-Pitch Listener (Intervals)</h1>
  <p>Try singing two notes together or playing an interval for 3 seconds</p>
  
  <button id="btn">🎤 Listen for 3 seconds</button>
  <div id="status">Ready</div>

  <script>
    const btn = document.getElementById('btn');
    const status = document.getElementById('status');
    let audioContext;

    function findPeaks(dataArray, sampleRate) {
      const peaks = [];
      const minFreq = 60;
      const maxFreq = 1500;
      const binSize = sampleRate / dataArray.length;

      for (let i = 1; i < dataArray.length / 2 - 1; i++) {
        const freq = i * binSize;
        if (freq < minFreq || freq > maxFreq) continue;

        if (dataArray[i] > dataArray[i-1] && dataArray[i] > dataArray[i+1]) {
          if (dataArray[i] > 0.001) {   // threshold
            peaks.push({ freq: Math.round(freq), magnitude: dataArray[i] });
          }
        }
      }

      // Sort by magnitude and take top 3
      peaks.sort((a, b) => b.magnitude - a.magnitude);
      return peaks.slice(0, 3).map(p => p.freq);
    }

    btn.onclick = async () => {
      btn.disabled = true;
      status.textContent = "Listening for 3 seconds... Sing an interval or chord";

      if (!audioContext) audioContext = new (window.AudioContext || window.webkitAudioContext)();

      try {
        const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
        const analyser = audioContext.createAnalyser();
        analyser.fftSize = 8192;                    // Higher resolution
        analyser.smoothingTimeConstant = 0.8;

        const source = audioContext.createMediaStreamSource(stream);
        source.connect(analyser);

        const buffer = new Float32Array(analyser.frequencyBinCount);

        setTimeout(() => {
          analyser.getFloatFrequencyData(buffer);
          stream.getTracks().forEach(t => t.stop());

          const detectedFreqs = findPeaks(buffer, audioContext.sampleRate);

          if (detectedFreqs.length > 0) {
            let text = "Detected frequencies:<br>";
            detectedFreqs.forEach(f => {
              text += `<span class="freq">→ ${f} Hz</span><br>`;
            });
            status.innerHTML = text;
            
            // Play all detected frequencies
            detectedFreqs.forEach((freq, i) => {
              setTimeout(() => playTone(freq, 0.35 - i*0.08), i * 150);
            });
          } else {
            status.textContent = "No clear pitches detected.";
          }

          btn.disabled = false;
        }, 3000);

      } catch(e) {
        status.textContent = "Microphone error.";
        btn.disabled = false;
      }
    };

    function playTone(freq, gainValue) {
      const now = audioContext.currentTime;
      const osc = audioContext.createOscillator();
      osc.type = 'triangle';           // Softer than sawtooth
      osc.frequency.value = freq;

      const gain = audioContext.createGain();
      gain.gain.setValueAtTime(gainValue, now);
      gain.gain.exponentialRampToValueAtTime(0.001, now + 10);

      const filter = audioContext.createBiquadFilter();
      filter.type = 'lowpass';
      filter.frequency.value = 6000;

      osc.connect(filter).connect(gain).connect(audioContext.destination);
      osc.start(now);
      osc.stop(now + 10);
    }
  </script>
</body>
</html>
