properties([[$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactNumToKeepStr: '2', numToKeepStr: '2']]])

node("master") {
    def projectName = "janus-impstar"

    def projectDir = pwd()+ "/${projectName}"
    def artifactDir = "${projectDir}/build"

    try {
        sh "mkdir -p '${projectDir}'"
        dir (projectDir) {
            stage("Checkout") {
                checkout scm
            }
            stage("configure") {
                sh "./configure --prefix=/usr --libdir=/usr/lib64 --shlibdir=/usr/lib64 --mandir=/usr/share/man --enable-static --disable-shared --enable-gpl --disable-everything --disable-ffplay --disable-ffserver --enable-runtime-cpudetect --enable-libvpx --enable-libx264 --enable-libopus --enable-libvorbis --disable-hwaccels --disable-vaapi --disable-opencl --enable-muxer=rawvideo --enable-muxer=rtp --enable-muxer=flv --enable-muxer=dash --enable-muxer=mp4 --enable-muxer=segment --enable-muxer=mjpeg --enable-muxer=webm --enable-muxer=mpegts --enable-muxer=hls --enable-muxer=image2 --enable-demuxer=rawvideo --enable-demuxer=sdp --enable-demuxer=rtp --enable-demuxer=flv --enable-demuxer=live_flv --enable-demuxer=m4v --enable-demuxer=mov --enable-demuxer=mjpeg --enable-demuxer=image2 --enable-decoder=rawvideo --enable-decoder=h264 --enable-decoder=png --enable-decoder=aac --enable-decoder=libvpx_vp8 --enable-decoder=vorbis --enable-decoder=libvorbis --enable-decoder=opus --enable-decoder=libopus --enable-encoder=libx264 --enable-encoder=libfaac --enable-encoder=aac --enable-encoder=mjpeg --enable-encoder=libvpx_vp8 --enable-encoder=vorbis --enable-encoder=libvorbis --enable-encoder=libopus --enable-encoder=png --enable-filter=fps --enable-filter=copy --enable-filter=scale --enable-filter=channelmap --enable-filter=anull --enable-filter=null --enable-filter=aformat --enable-filter=format --enable-filter=aresample --enable-filter=thumbnail --enable-filter=scale --enable-filter=tile --enable-bsf=h264_mp4toannexb --enable-protocol=rtmp --enable-protocol=rtp --enable-protocol=srtp --enable-protocol=tcp --enable-protocol=udp --enable-protocol=unix --enable-protocol=file --enable-protocol=http --enable-protocol=pipe --enable-parser=h264 --enable-parser=aac --enable-parser=opus --enable-parser=vp8 --extra-cflags='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --disable-stripping"
            }
            stage("make all") {
                sh "make"
            }
            stage("deploy") {
                archiveArtifacts artifacts: "ffmpeg", fingerprint: true
            }
            currentBuild.result = "SUCCESS"
        }
    } catch(e) {
        currentBuild.result = "FAILURE"
        throw e
    }
}