```geojson
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "id": 1,
      "properties": {
        "ID": 0
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          [
              [-90,35],
              [-90,30],
              [-85,30],
              [-85,35],
              [-90,35]
          ]
        ]
      }
    }
  ]
}
```

```stl
solid cube_corner
  facet normal 0.0 -1.0 0.0
    outer loop
      vertex 0.0 0.0 0.0
      vertex 1.0 0.0 0.0
      vertex 0.0 0.0 1.0
    endloop
  endfacet
  facet normal 0.0 0.0 -1.0
    outer loop
      vertex 0.0 0.0 0.0
      vertex 0.0 1.0 0.0
      vertex 1.0 0.0 0.0
    endloop
  endfacet
  facet normal -1.0 0.0 0.0
    outer loop
      vertex 0.0 0.0 0.0
      vertex 0.0 0.0 1.0
      vertex 0.0 1.0 0.0
    endloop
  endfacet
  facet normal 0.577 0.577 0.577
    outer loop
      vertex 1.0 0.0 0.0
      vertex 0.0 1.0 0.0
      vertex 0.0 0.0 1.0
    endloop
  endfacet
endsolid
```

```stl
#ifdef GL_FRAGMENT_PRECISION_HIGH
precision highp float;
#else
precision mediump float;
#endif

uniform vec2 resolution;
uniform float time;

float random(vec2 n) {
  return fract(sin(dot(n, vec2(12.9898, 4.1414))) * 43758.5453);
}

float noise(vec2 p){
  vec2 ip = floor(p);
  vec2 fp = fract(p);
  fp = fp * fp * (3.0-2.0*fp);
  float res = mix(mix(random(ip),random(ip+vec2(1.0,0.0)),fp.x),mix(random(ip+vec2(0.0,1.0)),random(ip+vec2(1.0,1.0)),fp.x),fp.y);
  return res;
}

void main() {
  vec2 uv = gl_FragCoord.xy/resolution.xy;
  uv -= 0.5;
  uv *= 2.0;
  float t = time * 0.1;
  float n = noise(uv * 10.0 + t);
  float m = noise(uv * 20.0 + t);
  float c = noise(uv * 30.0 + t);
  vec3 col = vec3(n, m, c);
  gl_FragColor = vec4(col, 1.0);
}
```
