---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# CDN을 사용하여 VoD(Video on Demand)를 제공하는 방법

이 안내서에서는 Linux-Nginx 원본에서 브라우저로 **HLS**를 통해 `.mp4` 컨텐츠를 VoD(Video on Demand)로 스트리밍하는 방법을 설명합니다. 

## 소개

HLS, MPEG-DASH 등의 수많은 형식으로 동영상을 스트리밍할 수 있습니다. 

사용 중인 설정에 대해서는 다음 다이어그램에서 개념적으로 표시됩니다.

![VoD(Video on Demand)용 IBM Cloud CDN](images/ibmcdn-vod-example-model.png)

또한 원본을 준비 장소로 사용합니다. 또한 이 작업을 위해 몇 가지 패키지가 필요합니다.

원본의 패키지 목록을 업데이트하는 것부터 시작합니다.
```
$ sudo apt-get update
```

## 동영상 파일 준비
이 안내서에서는 `ffmpeg`를 사용하여 동영상 파일을 준비합니다. 이는 다양한 명령을 사용하여 변환, 다중화, 다중화 해제, 필터링 등을 수행할 수 있는 강력한 멀티미디어 파일용 도구입니다.

먼저, `ffmpeg`가 필요합니다.
```
$ sudo apt-get -y install ffmpeg
```

HLS는 `.m3u8` 및 `.ts`라는 두 가지 유형의 파일과 함께 작동합니다. `.m3u8` 파일은 "재생 목록"으로 생각할 수 있습니다. 동영상 스트림의 시작에서 이 파일은 제일 먼저 페치되어야 합니다. 그런 다음 재생 목록이 동영상 플레이어에 페치해야 하는 동영상 단편에 대해 알리고 스트리밍된 내용을 성공적으로 재생하기 위한 방법에 관한 데이터를 제공합니다. `.ts` 파일은 동영상 "단편"입니다. 이러한 단편은 "재생 목록"에서 지정된 세부사항에 따라 동영상 플레이어에 의해 페치되고 재생됩니다.

이제 형식, 비트 전송률, 동영상 및 소스 `.mp4` 동영상의 오디오 스트림에 대한 기타 정보를 확인합니다.

```
$ ffprobe test-video.mp4
```

이 예에서 `test-video.mp4`에 대한 다음 스트림 정보를 고려하십시오.
  * 동영상 스트림 0
    * 형식: h264
    * 형식 프로파일: 고
    * 해상도: 1920x1080
    * 비트 전송률: 438KB/초
    * 프레임 전송률: 30.30fps
  * 오디오 스트림 0
    * 형식: aac
    * 샘플 전송률: 48000
    * 비트 전송률: 128K

이제 `test-video.mp4` 파일을 HLS용 형식으로 변환합니다.

```
$ ffmpeg -i test-video.mp4 -c:a aac -ar 48000 -b:a 128k -c:v h264 -profile:v main -crf 23 -g 61 -keyint_min 61 -sc_threshold 0 -b:v 5300k -maxrate 5300k -bufsize 10600k -hls_time 6 -hls_playlist_type vod test-video.m3u8
```

다음은 이 명령이 수행하는 내용을 분해한 것입니다.

|인수|영향|
|:---|:---|
| ffmpeg | `ffmpeg` 도구를 사용합니다. |
| -i test-video.mp4 | The source video is located at `test-video.mp4`. |
| -c:a acc | Use the acc audio codec for the output. |
| -ar 48000 | Set the audio sample rate to 48000 Hz for the output. |
| -b:a 128k | Set the audio bitrate to 128000 bits/second for the output. |
| -c:v h264 | Use the `h.264` video codec for the output. |
| -profile:v main | Use the "main" format profile of the selected codec for widest device support. |
| -crf 23 | Attempt to maintain the video quality with varying file size and bitrate.<br/>  The lower the CRF, the higher the quality and file size. |
| -g 61 -keyint_min 61 | Set a maximum and minimum.<br/> With the example source frame rate as 30.30, a keyframe should be <br/> inserted every 2 seconds (61 frames). |
| -sc_threshold 0 | Disable scene detection by `ffmpeg`.<br/> Prevents a second process that may insert extraneous keyframes into the output. |
| -b:v 5300k | Sets the output video stream's target bitrate to 5300000 bits/second. |
| -maxrate 5300k | Limits the maximum output video bitrate at<br/> the encoder to 5300000 bits/second, in case it varies. |
| -bufsize 10600k | Sets the `ffmpeg` video decoder buffer size to 10600000 bits.<br/>  With 5300k bitrate, the `ffmpeg` encoder should check and <br/> attempt to re-adjust the output bitrate back to the target bitrate for every 2 seconds of video. |
| -hls_time 6 | Attempt to target each output video fragment length to 6 seconds.<br/> Accumulates frames for at least 6 seconds of video, and then<br/> stops to break off a video fragment when it encounters the next keyframe. |
| -hls_playlist_type vod | Prepares the output `.m3u8` playlist file for video-on-demand (vod). |
| test-video.m3u8 | 출력 재생 목록/Manifest 파일의 이름을 `test-video.m3u8`에 지정합니다.<br/> 따라서 기본적으로 `test-video0.ts`, `test-video1.ts`, `test-video2.ts` 등과 유사하게<br/> 동영상 단편의 이름이 지정됩니다.|

`-` 옵션의 경우, 스트림이 지정되지 않으면 카테고리에 대한 "최적" 스트림이 선택됩니다.

예를 들어, 다음과 같습니다.
  * `-c:a`는 가장 많은 채널을 가진 오디오 스트림을 선택합니다.
  * `-c:v`는 최고의 해상도를 가진 동영상 스트림을 선택합니다.
  * `-c:a:0`은 오디오 스트림 0을 선택합니다.
  * `-c:v:0`은 동영상 스트림 0을 선택합니다.

이 안내서에서는 하나의 오디오 스트림과 하나의 동영상 스트림만이 `test-video.mp4` 예를 구성합니다. 따라서 차이는 고려되지 않습니다.

그런 다음 수많은 `.ts` 파일을 볼 수 있습니다. 또한 다음과 유사한 `.m3u8` 파일을 볼 수 있습니다.

```
$ cat test-video.m3u8
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-TARGETDURATION:7
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-PLAYLIST-TYPE:VOD
#EXTINF:6.039000,
test-video0.ts
#EXTINF:6.039000,
test-video1.ts
#EXTINF:6.039000,
test-video2.ts
#EXTINF:6.039000,
test-video3.ts
#EXTINF:6.039000,
test-video4.ts
#EXTINF:6.039000,
test-video5.ts
#EXTINF:6.039000,
test-video6.ts
#EXTINF:6.039000,
test-video7.ts
#EXTINF:6.039000,
test-video8.ts
#EXTINF:6.039000,
test-video9.ts
#EXTINF:6.039000,
test-video10.ts
#EXTINF:4.620000,
test-video11.ts
#EXT-X-ENDLIST
```
동영상 해상도 조절, 자막 작업, 보안 및 인증을 위한 동영상 단편에 대한 HLS AES 암호화 등의 복합 유스 케이스를 위해 `ffmpeg`에는 더 복잡하고 특정한 기능을 처리하는 수많은 인수 옵션이 있습니다. [ffmpeg's general documentation](https://ffmpeg.org/ffmpeg.html) 및 [documentation on specific formats such as HLS](https://ffmpeg.org/ffmpeg-formats.html#hls)에서 이러한 인수에 대한 설명을 볼 수 있습니다.

## 원본 준비
### 서버
이러한 HLS를 스트리밍하는 두 번째 도메인 아래의 추가 원본으로 이 서버를 사용 중인 경우, 서버가 잠재적인 브라우저 액세스에 대해 CORS 응답 헤더를 리턴하도록 구성해야 합니다.

HLS 파일을 원하는 디렉토리 또는 서브디렉토리 아래에 배치할 수 있습니다. 이 예에서는 HLS 파일을 `/usr/share/nginx/hls/` 아래에 배치합니다.

다음 Nginx 구성이 기본 사항을 포함합니다.

```
# Some configurations for this main context...

http {

    # Some configurations for this http context...

    server {

        # Some virtual host configurations here...

        location /hls {
            root /usr/share/nginx/;

            # CORS simple requests
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Expose-Headers' 'Content-Length, Content-Type';
            }

            # CORS preflight requests
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Allow-Methods' 'GET, OPTIONS';

                # Note: wildcard `Access-Control-Allow-Headers` may not be supported by all browsers, yet.
                add_header 'Access-Control-Allow-Headers' '*';

                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain; charset=utf-8';
                add_header 'Content-Length' 0;
                return 204;
            }

            try_files $uri =404;
        }

        # Some more virtual host configurations...
    }

    # Some more configurations for this http context...
}

# Some more configurations for this main context...
```

### 웹 페이지의 동영상 플레이어

모든 스트리밍 동영상 형식이 모든 애플리케이션에서 실행 가능한 것은 아닙니다. 이 안내서의 예에서는 HLS 및 CDN을 사용하여 스트리밍을 설정합니다.

예를 들어, Safari는 원시 HLS 재생을 지원합니다. 따라서 웹 페이지의 동영상 플레이어가 HTML5 `<video>` 요소를 사용하는 다음 예제와 같이 단순할 수 있습니다.

```
<!DOCTYPE html>
<html>
  <!-- Some HTML elements... -->

  <video src="https://cdn.example.com/hls/test-video.m3u8"></video>

  <!-- Some more HTML elements... -->
</html>
```

그러나 데스크탑 장치의 브라우저는 컨텐츠 스트림을 HTML5를 통해 재생하려면 내부에서 개발되었거나 신뢰할 수 있는 서드파티에서 추가된 JavaScript [Media Source Extensions](https://www.w3.org/TR/media-source/)의 지원이 필요할 수 있습니다.

## CDN 구성
이제 최적화된 처리량, 최소화된 대기 시간 및 향상된 성능으로 컨텐츠를 전세계적으로 제공할 수 있도록 원본을 CDN에 연결합니다.

먼저, CDN을 [주문](how-to-order.html#order-a-cdn)합니다.

다음으로 [CDN을 구성](how-to.html#updating-cdn-configuration-details)하거나 [원본을 추가](how-to.html#adding-origin-path-details)합니다.

마지막으로 `Optimize For` 아래에서 `Video on demand optimization`을 선택합니다.

![Optimize for Video on Demand](images/optimize-for-video-on-demand.png)
