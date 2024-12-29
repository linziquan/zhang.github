<!DOCTYPE html>
<html>​​
  <头>
    <元字符集=“utf-8”/>
    < title >风华正茂，书生意气</ title >title >风华正茂，书生意气</ title >
 
    <风格>
      html,html,
      身体{{
        边距：0像素；0像素；
        宽度：100 %；100 %；
        高度：100 %；100 %；
        溢出：隐藏；
        背景：#000；#000；
      }}
    </风格>
  </头>
  <正文>
    <有效


    >  </br>​​
    <有效


    >  </br>​​

      <div class =“选项卡”>div class = “选项卡”  >
        < div class = "选项卡标签" >div class = "选项卡标签"  >
          <span class =“tabs-label”>命令</spanspan class = “tabs-label”  >命令</ span
          > <span class =“tabs-label”>信息</spanspan class = “tabs-label”  >信息</ span
          > <span class =“tabs-标签”>分享</span>span class = “tabs- label ” >分享</span>
        </div>​​div>​​
 
        < div class = "选项卡面板" >div class = "选项卡面板" >
          < ul class = "选项卡面板命令" > </ ul >ul class = "选项卡面板命令" > </ ul >
        </div>
      </div>
    </div>
    <script>
      function initVars() {
        pi = Math.PI;
        ctx = canvas.getContext("2d");
        canvas.width = canvas.clientWidth;
        canvas.height = canvas.clientHeight;
        cx = canvas.width / 2;
        cy = canvas.height / 2;
        playerZ = -25;
        playerX =
          playerY =
          playerVX =
          playerVY =
          playerVZ =
          pitch =
          yaw =
          pitchV =
          yawV =
            0;
        scale = 600;
        seedTimer = 0;
        (seedInterval = 5), (seedLife = 100);
        gravity = 0.02;
        seeds = new Array();
        sparkPics = new Array();
        s = "https://cantelope.org/NYE/";
        for (i = 1; i <= 10; ++i) {
          sparkPic = new Image();
          sparkPic.src = s + "spark" + i + ".png";
          sparkPics.push(sparkPic);
        }
        sparks = new Array();
        pow1 = new Audio(s + "pow1.ogg");
        pow2 = new Audio(s + "pow2.ogg");
        pow3 = new Audio(s + "pow3.ogg");
        pow4 = new Audio(s + "pow4.ogg");
        frames = 0;
      }
 
      function rasterizePoint(x, y, z) {
        var p, d;
        x -= playerX;
        y -= playerY;
        z -= playerZ;
        p = Math.atan2(x, z);
        d = Math.sqrt(x * x + z * z);
        x = Math.sin(p - yaw) * d;
        z = Math.cos(p - yaw) * d;
        p = Math.atan2(y, z);
        d = Math.sqrt(y * y + z * z);
        y = Math.sin(p - pitch) * d;
        z = Math.cos(p - pitch) * d;
        var rx1 = -1000,
          ry1 = 1,
          rx2 = 1000,
          ry2 = 1,
          rx3 = 0,
          ry3 = 0,
          rx4 = x,
          ry4 = z,
          uc = (ry4 - ry3) * (rx2 - rx1) - (rx4 - rx3) * (ry2 - ry1);
        if (!uc) return { x: 0, y: 0, d: -1 };
        var ua = ((rx4 - rx3) * (ry1 - ry3) - (ry4 - ry3) * (rx1 - rx3)) / uc;
        var ub = ((rx2 - rx1) * (ry1 - ry3) - (ry2 - ry1) * (rx1 - rx3)) / uc;
        if (!z) z = 0.000000001;
        if (ua > 0 && ua < 1 && ub > 0 && ub < 1) {
          return {
            x: cx + (rx1 + ua * (rx2 - rx1)) * scale,
            y: cy + (y / z) * scale,
            d: Math.sqrt(x * x + y * y + z * z),
          };
        } else {
          return {
            x: cx + (rx1 + ua * (rx2 - rx1)) * scale,
            y: cy + (y / z) * scale,
            d: -1,
          };
        }
      }
 
      function spawnSeed() {
        seed = new Object();
        seed.x = -50 + Math.random() * 100;
        seed.y = 25;
        seed.z = -50 + Math.random() * 100;
        seed.vx = 0.1 - Math.random() * 0.2;
        seed.vy = -1.5; //*(1+Math.random()/2);
        seed.vz = 0.1 - Math.random() * 0.2;
        seed.born = frames;
        seeds.push(seed);
      }
 
      function splode(x, y, z) {
        t = 5 + parseInt(Math.random() * 150);
        sparkV = 1 + Math.random() * 2.5;
        type = parseInt(Math.random() * 3);
        switch (type) {
          case 0:
            pic1 = parseInt(Math.random() * 10);
            break;
          case 1:
            pic1 = parseInt(Math.random() * 10);
            do {
              pic2 = parseInt(Math.random() * 10);
            } while (pic2 == pic1);
            break;
          case 2:
            pic1 = parseInt(Math.random() * 10);
            do {
              pic2 = parseInt(Math.random() * 10);
            } while (pic2 == pic1);
            do {
              pic3 = parseInt(Math.random() * 10);
            } while (pic3 == pic1 || pic3 == pic2);
            break;
        }
        for (m = 1; m < t; ++m) {
          spark = new Object();
          spark.x = x;
          spark.y = y;
          spark.z = z;
          p1 = pi * 2 * Math.random();
          p2 = pi * Math.random();
          v = sparkV * (1 + Math.random() / 6);
          spark.vx = Math.sin(p1) * Math.sin(p2) * v;
          spark.vz = Math.cos(p1) * Math.sin(p2) * v;
          spark.vy = Math.cos(p2) * v;
          switch (type) {
            case 0:
              spark.img = sparkPics[pic1];
              break;
            case 1:
              spark.img = sparkPics[parseInt(Math.random() * 2) ? pic1 : pic2];
              break;
            case 2:
              switch (parseInt(Math.random() * 3)) {
                case 0:
                  spark.img = sparkPics[pic1];
                  break;
                case 1:
                  spark.img = sparkPics[pic2];
                  break;
                case 2:
                  spark.img = sparkPics[pic3];
                  break;
              }
              break;
          }
          spark.radius = 25 + Math.random() * 50;
          spark.alpha = 1;
          spark.trail = new Array();
          sparks.push(spark);
        }
        switch (parseInt(Math.random() * 4)) {
          case 0:
            pow = new Audio(s + "pow1.ogg");
            break;
          case 1:
            pow = new Audio(s + "pow2.ogg");
            break;
          case 2:
            pow = new Audio(s + "pow3.ogg");
            break;
          case 3:
            pow = new Audio(s + "pow4.ogg");
            break;
        }
        d = Math.sqrt(
          (x - playerX) * (x - playerX) +
            (y - playerY) * (y - playerY) +
            (z - playerZ) * (z - playerZ)
        );
        pow.volume = 1.5 / (1 + d / 10);
        pow.play();
      }
 
      function doLogic() {
        if (seedTimer < frames) {
          seedTimer = frames + seedInterval * Math.random() * 10;
          spawnSeed();
        }
        for (i = 0; i < seeds.length; ++i) {
          seeds[i].vy += gravity;
          seeds[i].x += seeds[i].vx;
          seeds[i].y += seeds[i].vy;
          seeds[i].z += seeds[i].vz;
          if (frames - seeds[i].born > seedLife) {
            splode(seeds[i].x, seeds[i].y, seeds[i].z);
            seeds.splice(i, 1);
          }
        }
        for (i = 0; i < sparks.length; ++i) {
          if (sparks[i].alpha > 0 && sparks[i].radius > 5) {
            sparks[i].alpha -= 0.01;
            sparks[i].radius /= 1.02;
            sparks[i].vy += gravity;
            point = new Object();
            point.x = sparks[i].x;
            point.y = sparks[i].y;
            point.z = sparks[i].z;
            if (sparks[i].trail.length) {
              x = sparks[i].trail[sparks[i].trail.length - 1].x;
              y = sparks[i].trail[sparks[i].trail.length - 1].y;
              z = sparks[i].trail[sparks[i].trail.length - 1].z;
              d =
                (point.x - x) * (point.x - x) +
                (point.y - y) * (point.y - y) +
                (point.z - z) * (point.z - z);
              if (d > 9) {
                sparks[i].trail.push(point);
              }
            } else {
              sparks[i].trail.push(point);
            }
            if (sparks[i].trail.length > 5) sparks[i].trail.splice(0, 1);
            sparks[i].x += sparks[i].vx;
            sparks[i].y += sparks[i].vy;
            sparks[i].z += sparks[i].vz;
            sparks[i].vx /= 1.075;
            sparks[i].vy /= 1.075;
            sparks[i].vz /= 1.075;
          } else {
            sparks.splice(i, 1);
          }
        }
        p = Math.atan2(playerX, playerZ);
        d = Math.sqrt(playerX * playerX + playerZ * playerZ);
        d += Math.sin(frames / 80) / 1.25;
        t = Math.sin(frames / 200) / 40;
        playerX = Math.sin(p + t) * d;
        playerZ = Math.cos(p + t) * d;
        yaw = pi + p + t;
      }
 
      function rgb(col) {
        var r = parseInt((0.5 + Math.sin(col) * 0.5) * 16);
        var g = parseInt((0.5 + Math.cos(col) * 0.5) * 16);
        var b = parseInt((0.5 - Math.sin(col) * 0.5) * 16);
        return "#" + r.toString(16) + g.toString(16) + b.toString(16);
      }
 
      function draw() {
        ctx.clearRect(0, 0, cx * 2, cy * 2);
 
        ctx.fillStyle = "#ff8";
        for (i = -100; i < 100; i += 3) {
          for (j = -100; j < 100; j += 4) {
            x = i;
            z = j;
            y = 25;
            point = rasterizePoint(x, y, z);
            if (point.d != -1) {
              size = 250 / (1 + point.d);
              d = Math.sqrt(x * x + z * z);
              a = 0.75 - Math.pow(d / 100, 6) * 0.75;
              if (a > 0) {
                ctx.globalAlpha = a;
                ctx.fillRect(
                  point.x - size / 2,
                  point.y - size / 2,
                  size,
                  size
                );
              }
            }
          }
        }
        ctx.globalAlpha = 1;
        for (i = 0; i < seeds.length; ++i) {
          point = rasterizePoint(seeds[i].x, seeds[i].y, seeds[i].z);
          if (point.d != -1) {
            size = 200 / (1 + point.d);
            ctx.fillRect(point.x - size / 2, point.y - size / 2, size, size);
          }
        }
        point1 = new Object();
        for (i = 0; i < sparks.length; ++i) {
          point = rasterizePoint(sparks[i].x, sparks[i].y, sparks[i].z);
          if (point.d != -1) {
            size = (sparks[i].radius * 200) / (1 + point.d);
            if (sparks[i].alpha < 0) sparks[i].alpha = 0;
            if (sparks[i].trail.length) {
              point1.x = point.x;
              point1.y = point.y;
              switch (sparks[i].img) {
                case sparkPics[0]:
                  ctx.strokeStyle = "#f84";
                  break;
                case sparkPics[1]:
                  ctx.strokeStyle = "#84f";
                  break;
                case sparkPics[2]:
                  ctx.strokeStyle = "#8ff";
                  break;
                case sparkPics[3]:
                  ctx.strokeStyle = "#fff";
                  break;
                case sparkPics[4]:
                  ctx.strokeStyle = "#4f8";
                  break;
                case sparkPics[5]:
                  ctx.strokeStyle = "#f44";
                  break;
                case sparkPics[6]:
                  ctx.strokeStyle = "#f84";
                  break;
                case sparkPics[7]:
                  ctx.strokeStyle = "#84f";
                  break;
                case sparkPics[8]:
                  ctx.strokeStyle = "#fff";
                  break;
                case sparkPics[9]:
                  ctx.strokeStyle = "#44f";
                  break;
              }
              for (j = sparks[i].trail.length - 1; j >= 0; --j) {
                point2 = rasterizePoint(
                  sparks[i].trail[j].x,
                  sparks[i].trail[j].y,
                  sparks[i].trail[j].z
                );
                if (point2.d != -1) {
                  ctx.globalAlpha =
                    ((j / sparks[i].trail.length) * sparks[i].alpha) / 2;
                  ctx.beginPath();
                  ctx.moveTo(point1.x, point1.y);
                  ctx.lineWidth =
                    1 +
                    (sparks[i].radius * 10) /
                      (sparks[i].trail.length - j) /
                      (1 + point2.d);
                  ctx.lineTo(point2.x, point2.y);
                  ctx.stroke();
                  point1.x = point2.x;
                  point1.y = point2.y;
                }
              }
            }
            ctx.globalAlpha = sparks[i].alpha;
            ctx.drawImage(
              sparks[i].img,
              point.x - size / 2,
              point.y - size / 2,
              size,
              size
            );
          }
        }
      }
 
      function frame() {
        if (frames > 100000) {
          seedTimer = 0;
          frames = 0;
        }
        frames++;
        draw();
        doLogic();
        requestAnimationFrame(frame);
      }
 
      window.addEventListener("resize", () => {
        canvas.width = canvas.clientWidth;
        canvas.height = canvas.clientHeight;
        cx = canvas.width / 2;
        cy = canvas.height / 2;
      });
 
      initVars();
      frame();
    </script>
    <script src="js/index.js"></script>
    <div id="page_end_html">
      <!--放大图片-->
      <link
        rel="stylesheet"
        type="text/css"
        href="https://blog-static.cnblogs.com/files/zouwangblog/zoom.css"
      />
      <script src="https://cdn.bootcss.com/jquery/1.8.3/jquery.min.js"></script>
      <script src="https://cdn.bootcss.com/bootstrap/3.2.0/js/transition.js"></script>
      <script src="https://blog-static.cnblogs.com/files/zouwangblog/zoom.js"></script>
      <script type="text/javascript">
        $("#cnblogs_post_body img").attr("data-action", "zoom");
      </script>
      <!--放大图片end-->
    
      <!--鼠标特效-->
      <script src="https://blog-static.cnblogs.com/files/zouwangblog/mouse-click.js"></script>
      <canvas
        width="1777"
        height="841"
        style="
          position: fixed;
          left: 0px;
          top: 0px;
          z-index: 2147483647;
          pointer-events: none;
        "
      ></canvas>
      <!--鼠标特效 end-->
      <!-- require APlayer -->
      <link
        rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.css"
      />
      <script src="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.js"></script>
      <!-- require MetingJS -->
      <script src="https://cdn.jsdelivr.net/npm/meting@2/dist/Meting.min.js"></script>
    
      <!-- // 随机线条 -->
      <script>
        !(function () {
          function n(n, e, t) {
            return n.getAttribute(e) || t;
          }
          function e(n) {
            return document.getElementsByTagName(n);
          }
          function t() {
            var t = e("script"),
              o = t.length,
              i = t[o - 1];
            return {
              l: o,
              z: n(i, "zIndex", -1),
              o: n(i, "opacity", 0.6),
              c: n(i, "color", "148,0,211"),
              n: n(i, "count", 99),
            };
          }
          function o() {
            (a = m.width =
              window.innerWidth ||
              document.documentElement.clientWidth ||
              document.body.clientWidth),
              (c = m.height =
                window.innerHeight ||
                document.documentElement.clientHeight ||
                document.body.clientHeight);
          }
          function i() {
            r.clearRect(0, 0, a, c);
            var n, e, t, o, m, l;
            s.forEach(function (i, x) {
              for (
                i.x += i.xa,
                  i.y += i.ya,
                  i.xa *= i.x > a || i.x < 0 ? -1 : 1,
                  i.ya *= i.y > c || i.y < 0 ? -1 : 1,
                  r.fillRect(i.x - 0.5, i.y - 0.5, 1, 1),
                  e = x + 1;
                e < u.length;
                e++
              )
                (n = u[e]),
                  null !== n.x &&
                    null !== n.y &&
                    ((o = i.x - n.x),
                    (m = i.y - n.y),
                    (l = o * o + m * m),
                    l < n.max &&
                      (n === y &&
                        l >= n.max / 2 &&
                        ((i.x -= 0.03 * o), (i.y -= 0.03 * m)),
                      (t = (n.max - l) / n.max),
                      r.beginPath(),
                      (r.lineWidth = t / 2),
                      (r.strokeStyle = "rgba(" + d.c + "," + (t + 0.2) + ")"),
                      r.moveTo(i.x, i.y),
                      r.lineTo(n.x, n.y),
                      r.stroke()));
            }),
              x(i);
          }
          var a,
            c,
            u,
            m = document.createElement("canvas"),
            d = t(),
            l = "c_n" + d.l,
            r = m.getContext("2d"),
            x =
              window.requestAnimationFrame ||
              window.webkitRequestAnimationFrame ||
              window.mozRequestAnimationFrame ||
              window.oRequestAnimationFrame ||
              window.msRequestAnimationFrame ||
              function (n) {
                window.setTimeout(n, 1e3 / 45);
              },
            w = Math.random,
            y = { x: null, y: null, max: 2e4 };
          (m.id = l),
            (m.style.cssText =
              "position:fixed;top:0;left:0;z-index:" + d.z + ";opacity:" + d.o),
            e("body")[0].appendChild(m),
            o(),
            (window.onresize = o),
            (window.onmousemove = function (n) {
              (n = n || window.event), (y.x = n.clientX), (y.y = n.clientY);
            }),
            (window.onmouseout = function () {
              (y.x = null), (y.y = null);
            });
          for (var s = [], f = 0; d.n > f; f++) {
            var h = w() * a,
              g = w() * c,
              v = 2 * w() - 1,
              p = 2 * w() - 1;
            s.push({ x: h, y: g, xa: v, ya: p, max: 6e3 });
          }
          (u = s.concat([y])),
            setTimeout(function () {
              i();
            }, 100);
        })();
      </script>
    
      <!-- 雪花特效 -->
      <script type="text/javascript">
        (function ($) {
          $.fn.snow = function (options) {
            var $flake = $('<div id="snowbox" />')
                .css({ position: "absolute", "z-index": "9999", top: "-50px" })
                .html("❄"),
              documentHeight = $(document).height(),
              documentWidth = $(document).width(),
              defaults = {
                minSize: 10,
                maxSize: 20,
                newOn: 1000,
                flakeColor:
                  "#00CED1" /* 此处可以定义雪花颜色，若要白色可以改为#FFFFFF */,
              },
              options = $.extend({}, defaults, options);
            var interval = setInterval(function () {
              var startPositionLeft = Math.random() * documentWidth - 100,
                startOpacity = 0.5 + Math.random(),
                sizeFlake = options.minSize + Math.random() * options.maxSize,
                endPositionTop = documentHeight - 200,
                endPositionLeft = startPositionLeft - 500 + Math.random() * 500,
                durationFall = documentHeight * 10 + Math.random() * 5000;
              $flake
                .clone()
                .appendTo("body")
                .css({
                  left: startPositionLeft,
                  opacity: startOpacity,
                  "font-size": sizeFlake,
                  color: options.flakeColor,
                })
                .animate(
                  {
                    top: endPositionTop,
                    left: endPositionLeft,
                    opacity: 0.2,
                  },
                  durationFall,
                  "linear",
                  function () {
                    $(this).remove();
                  }
                );
            }, options.newOn);
          };
        })(jQuery);
        $(function () {
          $.fn.snow({
            minSize: 5 /* 定义雪花最小尺寸 */,
            maxSize: 80 /* 定义雪花最大尺寸 */,
            newOn: 200 /* 定义密集程度，数字越小越密集 */,
          });
        });
      </script>
  </body>
</html>