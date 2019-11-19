<template>
  <div>
    <svg :width="`${width}px`" :height="`${height}px`" :viewBox="`0 0 ${width} ${height}`" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path v-for="(triangle, j) in shape.triangles" :key="`triangle-${j}`" :d="triangle.path" :fill="triangle.bgColor" />
      <path :d="shape.inner" :stroke="innerStrokeColor" :stroke-width="innerStrokeWidth" />
      <path :d="shape.hull" :stroke="hullStrokeColor" :stroke-width="hullStrokeWidth" />
      <path :d="shape.points" :stroke="pointStrokeColor" :stroke-width="pointStrokeWidth" />
    </svg>
  </div>
</template>

<script>
import { Delaunay } from 'd3-delaunay';
import _random from 'lodash/random';

export default {
    name: 'shape',
    props: {
        pointCount: { type: Number, default: 7 },
        height: { type: Number, default: 160 },
        width: { type: Number, default: 200 },
        pointRadius: { type: Number, default: 6 },
        pointStrokeWidth: { type: Number, default: 2 },
        pointStrokeColor: { type: String, default: 'rgba(255, 0, 153, 1)' },
        innerStrokeWidth: { type: Number, default: 1 },
        innerStrokeColor: { type: String, default: 'rgba(255, 0, 153, 1)' },
        hullStrokeWidth: { type: Number, default: 2 },
        hullStrokeColor: { type: String, default: 'rgba(255, 255, 255, 1)' },
        triangleColours: {
            type: Array,
            default() {
                return [
                  'none',
                  '#B30083',
                  '#E10090',
                  '#B30174',
                  '#D10182',
                  '#8C0260',
              ]
            },
        },
    },
    computed: {
        innerPadding () {
            return this.pointRadius + (this.pointStrokeWidth / 2);
        },
        minX () { return this.innerPadding; },
        maxX () { return this.width - this.innerPadding; },
        minY () { return this.innerPadding; },
        maxY () { return this.height - this.innerPadding; },
        shape () {
            let points = [];

            // points.push([this.maxX, this.minY], [this.maxX, this.maxY], [this.minX, this.maxY], [this.minX, this.minY]);

            // Point along the top
            points.push([Math.round(_random(this.minX, this.maxX) / 10) * 10, this.minY]);
            // Random point along the bottom
            points.push([Math.round(_random(this.minX, this.maxX) / 10) * 10, this.maxY]);

            // Random points in between
            for (let i = 0; i < (this.pointCount) - 2; i++) {
                let x = _random(this.minX, this.maxX);
                let y = _random(this.minY, this.maxY);

                x = Math.round(x / 10) * 10;
                y = Math.round(y / 10) * 10;

                points.push([x, y]);
            }

            let delaunay = Delaunay.from(points);

            return {
                points: delaunay.renderPoints(null, this.pointRadius),
                inner: delaunay.render(),
                hull: delaunay.renderHull(),
                triangles: Array.from(delaunay.trianglePolygons()).map((value, key) => {
                    let colorIndex = _random(this.triangleColours.length - 1);
                    return {
                        path: delaunay.renderTriangle(key),
                        bgColor: this.triangleColours[colorIndex],
                    };
                }),
            };
        },
    },
};
</script>
