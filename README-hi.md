# GPUsion

> GPU का आभास — हर भारतीय के लिए AI एक्सेलेरेशन

आपके लैपटॉप को लगता है कि उसमें GPU है। वास्तव में ऐसा नहीं है। यही इसका उद्देश्य है।

---

## GPUsion क्या है?

GPUsion एक ओपन-सोर्स Windows ड्राइवर है जो आपके CPU और ऑपरेटिंग सिस्टम को यह विश्वास दिलाता है कि सिस्टम में एक GPU मौजूद है। इसके बाद यह सभी AI inference workloads को एक अत्यधिक अनुकूलित CPU इंजन पर चलाता है।

आपके द्वारा इंस्टॉल किया गया हर AI ऐप — Ollama, Whisper, LM Studio — Device Manager में एक GPU देखता है और चलता है।

GPU की आवश्यकता नहीं है। कोई cloud subscription नहीं। जटिल configuration की आवश्यकता नहीं।

## हम किस समस्या का समाधान कर रहे हैं?

भारत में ₹30,000–₹60,000 की कीमत वाले 300 मिलियन+ लैपटॉप हैं।
इनमें से किसी में भी dedicated GPU नहीं है।
हर गंभीर local AI workload के लिए GPU की आवश्यकता होती है।

| आप क्या चलाना चाहते हैं | इसके लिए क्या चाहिए | इसकी लागत |
|---|---|---|
| Llama 3 8B को locally चलाना | 8GB VRAM | ₹25,000+ GPU |
| Stable Diffusion | कम से कम RTX 3060 | ₹22,000+ GPU |
| Whisper (speech → text) | GPU बेहतर है | CPU पर 5× धीमा |
| Local coding assistant | real-time के लिए GPU | ₹800–2,000/माह cloud |

समस्या intelligence की नहीं है। समस्या access की है।
GPUsion इस बाधा को दूर करता है।

## यह कैसे काम करता है

```text
┌─────────────────────────────────────────────────────────┐
│  आपका AI ऐप   (Ollama / Whisper / LM Studio / आदि)    │
│              सामान्य GPU APIs को कॉल करता है              │
└─────────────────────────────────────────────────────────┘
                       │
                       ▼
             DirectML / Vulkan / OpenCL calls
                       │
                       ▼
┌───────────────────────────────────────────────┐
│              GPUSION VIRTUAL DRIVER           │
│      Windows इसे एक वास्तविक GPU adapter        │
│      मानता है                                   │
│      "GPUsion Virtual Adapter — 8GB VRAM"     │
│      Device Manager में दिखाई देता है।             │
│      DXGI enumeration. WDDM 2.x compatible.   │
└───────────────────────────────────────────────┘
                       │
                       ▼
               inference workloads
                       │
                       ▼
┌───────────────────────────────────────────────┐
│                 INFERENCE ENGINE              │
│      llama.cpp • ONNX Runtime • AVX2/AVX-512  │
│      SIMD                                     │
│      INT4/INT8 quantization • CPU RAM को      │
│      VRAM के रूप में इस्तेमाल किया जाता है          │
└───────────────────────────────────────────────┘
```
**भ्रम सिर्फ driver के स्तर पर है। Performance असली है।**

