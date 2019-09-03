# WEBRTC FEC 动态保护策略

## 限制

* 每一组最多保护48个包（受限与RTP协议）

## 参数

* FEC 包个数 - 由拥塞控制设置
    + key
    + delta
    + 掩码设置
    
* FEC 掩码（保护质量的关键因素）
    + webrtc已经设计好所有保护组合的掩码
    + random（常用）
    + bursty
    
## FEC 掩码设计 [可复用]

* [random table](https://cs.chromium.org/chromium/src/third_party/webrtc/modules/rtp_rtcp/source/fec_private_tables_random.h)
* [bursty table](https://cs.chromium.org/chromium/src/third_party/webrtc/modules/rtp_rtcp/source/fec_private_tables_bursty.h)

* [hamming distance](https://en.wikipedia.org/wiki/Block_code#Error_detection_and_correction_properties)