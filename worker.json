addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request));
});

async function handleRequest(request) {
  const url = new URL(request.url);
  const pathname = url.pathname.slice(1);

  const channels = {
    "01": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/alvin-music.m3u8",
    "02": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/ftvturkiye.m3u8",
    "03": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/tomandjerry.m3u8",
    "04": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/t-tv.m3u8",
    "05": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/ksunal.m3u8",
    "06": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/hababam.m3u8",
    "07": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/pepe.m3u8",
    "08": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/b-kerimoff-masa-veayi.m3u8",
    "09": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/fanatikprimekurtlarvadisi.m3u8",
    "10": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/fanatikmaxyesilcam.m3u8",
    "11": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/yansinematv.m3u8",
    "12": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/casperscoobydooi.m3u8",
    "13": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/by-kerimoff-yesilcam.m3u8",
    "14": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/mrbeanaz.m3u8",
    "15": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/fanatikkidscaillou.m3u8",
    "16": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/film-2.m3u8",
    "17": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/by-kerimoff-keloglan.m3u8",
    "18": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/underworld.m3u8",
    "19": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/fanatikmaxkorku.m3u8",
    "20": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/fanatikmaxdeadpool.m3u8",
    "21": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/ben10.m3u8",
    "22": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/alvin-ovcu.m3u8",
    "23": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/azerimuzik.m3u8",
    "24": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/kanal-65.m3u8",
    "25": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/korkutv.m3u8",
    "26": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/masailekocaayi.m3u8",
    "27": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/megatvaz.m3u8",
    "28": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/now-music.m3u8",
    "29": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/rihatmusic.m3u8",
    "30": "https://raw.githubusercontent.com/sahind01/mutcats/refs/heads/main/catcast/turkuzplay1.m3u8",

    // 🔥 TROX
    "31": "https://trox.cinemax6.com:3545/stream/play.m3u8",

    // 🔥 SENİN CATCAST TOKEN LINKİN
    "32": "https://s.catcast.tv/content/50823/index.m3u8?token=edc90861a442bb2530b1bf43eb25dc92"
  };

  // 🎬 M3U8
  if (pathname.endsWith('.m3u8')) {
    const channelId = pathname.replace('.m3u8', '');
    const targetM3u8 = channels[channelId];

    if (!targetM3u8) {
      return new Response(`Kanal bulunamadı: ${channelId}`, { status: 404 });
    }

    try {
      const resp = await fetch(targetM3u8, {
        headers: {
          'User-Agent': 'Mozilla/5.0',
          'Referer': new URL(targetM3u8).origin + '/'
        }
      });

      let body = await resp.text();

      // 🔁 SEGMENT REWRITE (DÜZELTİLDİ)
      body = body.replace(
        /^(?!#).+\.(ts|m4s|mp4|m3u8|key)(\?.*)?$/gmi,
        (line) => {
          let u = line.trim();
          if (!u.startsWith('http')) {
            u = new URL(u, targetM3u8).href;
          }
          const encoded = btoa(u).replace(/\+/g, '-').replace(/\//g, '_').replace(/=+$/, '');
          return `/proxy/${channelId}/${encoded}`;
        }
      );

      // 🔐 KEY REWRITE
      body = body.replace(
        /(#EXT-X-KEY:[^,]+URI=")([^"]+)(".*)/gi,
        (full, p1, uri, p3) => {
          let keyUrl = uri;
          if (!keyUrl.startsWith('http')) {
            keyUrl = new URL(keyUrl, targetM3u8).href;
          }
          const encoded = btoa(keyUrl).replace(/\+/g, '-').replace(/\//g, '_').replace(/=+$/, '');
          return `${p1}/proxy/${channelId}/${encoded}${p3}`;
        }
      );

      return new Response(body, {
        headers: {
          'Content-Type': 'application/vnd.apple.mpegurl',
          'Access-Control-Allow-Origin': '*'
        }
      });

    } catch (e) {
      return new Response("Playlist alınamadı", { status: 500 });
    }
  }

  // 📡 PROXY
  if (pathname.startsWith('proxy/')) {
    const parts = pathname.split('/');
    const encoded = parts[3];

    let urlDecoded = encoded.replace(/-/g, '+').replace(/_/g, '/');
    while (urlDecoded.length % 4) urlDecoded += '=';

    const realUrl = atob(urlDecoded);

    const resp = await fetch(realUrl, {
      headers: {
        'User-Agent': 'Mozilla/5.0',
        'Referer': new URL(realUrl).origin + '/',
        'Origin': new URL(realUrl).origin
      }
    });

    return new Response(resp.body, {
      headers: {
        'Access-Control-Allow-Origin': '*'
      }
    });
  }

  return new Response("NeoFlix HLS Proxy aktif 🚀");
}