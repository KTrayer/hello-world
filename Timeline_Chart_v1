import React, { useRef, useEffect, useState } from 'react';
import {
    select,
    axisBottom,
    axisLeft,
    scaleLinear,
    scaleBand,
    min,
    max,
    scaleTime,
    //autoType
  } from "d3";

  import viewPortResizeObjserver from './internalFunctions/viewPortResizeObserver';



const Timeline_Chart_v1 = () => {

    const parentRef = useRef();
    const svgRef = useRef();
    const dimensions = viewPortResizeObjserver(parentRef); //custom hook for tracking browser size

    const [data, setData] = useState([
        {name:'prx1',start:"2020-11-01 00:00",end:"2020-11-15 00:00", startDay:2, endDay: 15, value: 6 }
        ,{name:'prx2',start:"2020-10-01 00:00",end:"2020-11-10 00:00", startDay:22, endDay: 45 , value: 9  }
        ,{name:'prx3',start:"2020-11-22 00:00",end:"2020-12-15 00:00", startDay:10, endDay: 15 , value: 16  }
        ,{name:'prx4',start:"2020-11-06 00:00",end:"2020-11-17 00:00", startDay:-4, endDay: 15 , value: 2  }
    ])
   

    useEffect(() => {
        const svg = select(svgRef.current);
        
        if(!dimensions) return;

            const minDate = new Date("2020-11-01 00:00")
            const maxDate = new Date("2020-11-30 23:59")

        // This scale builds out calender days from the minDate to the maxDate
        // I dont know how to use dates to tie my Y coordinates to this scale   
        const xScaleTime = scaleTime()
          .domain([minDate, maxDate]) //todo - import data from parent 
          .range([0, dimensions.width])
          //.padding(0.5) //space between bars

        // To make this semi work i created another xScale that just uses numbers so I can tie my Y coordinates to this scale
        // This is not needed if the above is able to work
        const xScale = scaleLinear()
          .domain([0, 30]) // [0, 65 (example)]
          .range([0, dimensions.height])


          
        //yScale nothing special
        const yScale = scaleBand()
            .paddingInner(0.5)
            .domain(data.map((dataPoint, index) => index))
            .range([0, dimensions.height]);

        //Drawing the xAxis using the time scale
        const xAxis = axisBottom(xScaleTime);

        //Aligns scale
        svg
            .select('.x-axis')
            .style("transform", `translateY(${dimensions.height}px)`)
            .call(xAxis);
            

        //Draws Bars, this is taken from your race bar chart
        svg
            .selectAll('.bar')
            .data(data, (entry, index) => entry.name)
            .join(enter =>
                enter.append("rect").attr("y", (entry, index) => yScale(index))
              )
            .attr('class','.bar')
            .attr('x', entry => xScale(entry.startDay) )
            .attr('height', yScale.bandwidth())
            .transition()
            .attr('width', entry => xScale(entry.endDay)  )
            .attr('y', (data, index) => yScale(index) )
            .attr('fill', 'red');
        
        //Draws Labels, this is taken from your race bar chart
        svg 
            .selectAll(".label")
            .data(data, (entry, index) => entry.name)
            .join(enter =>
                enter
                  .append("text")
                  .attr(
                    "y",
                    (entry, index) => yScale(index) + yScale.bandwidth() / 2 + 5
                  )
              )
            .text(entry => entry.name)
            .attr("class", "label")
            .attr("x", 10)
            .transition()
            .attr("y", (entry, index) => yScale(index) + yScale.bandwidth() / 2 + 5)

    },[data,dimensions])


    return (
        <div className='lineChart_v1'ref={parentRef} style={{width:"100%" , height:"100%"}}> 
            <svg ref={svgRef} style={{width:'100%' , height:'100%'}}>
                <g className="x-axis" />
            </svg>
        </div>
    );
  
    

}
Timeline_Chart_v1.propTypes = {
  
   
}

export default Timeline_Chart_v1;
