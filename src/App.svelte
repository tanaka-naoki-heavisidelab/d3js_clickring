<script lang="ts">
  import { onMount } from "svelte";
  import * as d3 from "d3";

  let _svg: d3.Selection<d3.BaseType, unknown, HTMLElement, any>;
  let svg = _svg as unknown | SVGElement;

  onMount(() => {
    _svg = d3.select("svg");

    _svg
      .attr("width", 400)
      .attr("height", 400)
      .attr("style", "background: #AAA")
      .on("click", function (event: PointerEvent) {
        viewclick(event);
      });

    function viewclick(event: PointerEvent) {
      for (var l = 0; l < 4; l++) {
        let color = d3.rgb(
          Math.floor(Math.random() * 255) + 1,
          Math.floor(Math.random() * 255) + 1,
          Math.floor(Math.random() * 255) + 1
        );
        let color_id = color as unknown as string;
        let circle = _svg.append("circle");
        circle
          .attr("cx", event.offsetX)
          .attr("cy", event.offsetY)
          .attr("r", 5)
          .attr("stroke", color_id)
          .attr("stroke-width", 5)
          .style("fill", "none");

        for (var i = 0; i < 11; i++) {
          circle
            .transition()
            .delay(300 * l + 50 * i)
            .duration(50)
            .attr("r", 10 + 10 * i)
            .attr("opacity", 1 - 0.1 * i)
            .transition()
            .delay(2000)
            .remove();
        }
      }
    }
  });
</script>

<svg bind:this={svg} />
