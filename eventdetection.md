
# Calculating recurrence scores in time series


```julia
using CABLAB
using ImageMagick #for inline Map plots
```

We define the path to the datacube, choose some variables and read a geographical region into memory.


```julia
c             = Cube("/Net/Groups/BGI/scratch/fgans/cubecopy/datacube/")
vars          = ["Emission","SoilMoisture","t2m"];
cdata         = getCubeData(c,latitude=(50,56), longitude=(20,25),variable=vars);
```

Here starts the actual processing step. Note that each function call has the result of the previous call as its argument. The following processing steps are applied:

- gap Filling with the mean seasonal cycle
- calculating anomalies, i.e. subtract the mean seasonal cycle
- normalize the different variables to unit variance
- caculate recurrence scores to detect outliers

All these processing steps are implemented in the DAT.


```julia
@time cube_filled     = mapCube(gapFillMSC,cdata);
```

      0.328695 seconds (135.53 k allocations: 17.844 MB, 4.22% gc time)



```julia
@time cube_anomalies  = mapCube(removeMSC,cube_filled);
```

      0.110225 seconds (158.71 k allocations: 8.257 MB)



```julia
@time cube_normalized = mapCube(normalize,cube_anomalies);
```

      0.118978 seconds (157.26 k allocations: 8.147 MB, 10.29% gc time)



```julia
@time scores          = mapCube(recurrences,cube_normalized,5.0,5);
```

      1.319626 seconds (778.52 k allocations: 33.989 MB, 0.58% gc time)


Here follow some interactive time series plots of the different variables, anomalies, and the resulting scores. 3 Extreme events would be detected here. 


```julia
plotTS(cube_filled)
```






















<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     xmlns:gadfly="http://www.gadflyjl.org/ns"
     version="1.2"
     width="141.42mm" height="100mm" viewBox="0 0 141.42 100"
     stroke="none"
     fill="#000000"
     stroke-width="0.3"
     font-size="3.88"

     id="img-2f634ffe">
<g class="plotroot xscalable yscalable" id="img-2f634ffe-1">
  <g font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" fill="#564A55" stroke="#000000" stroke-opacity="0.000" id="img-2f634ffe-2">
    <text x="66.8" y="88.39" text-anchor="middle" dy="0.6em">x</text>
  </g>
  <g class="guide xlabels" font-size="2.82" font-family="'PT Sans Caption','Helvetica Neue','Helvetica',sans-serif" fill="#6C606B" id="img-2f634ffe-3">
    <text x="-111.58" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">Jan 1, 1980</text>
    <text x="-79.13" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1985</text>
    <text x="-46.7" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1990</text>
    <text x="-14.28" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1995</text>
    <text x="18.15" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2000</text>
    <text x="50.6" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2005</text>
    <text x="83.03" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2010</text>
    <text x="115.45" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2015</text>
    <text x="147.88" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2020</text>
    <text x="180.33" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2025</text>
    <text x="212.76" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2030</text>
    <text x="245.19" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2035</text>
    <text x="-111.58" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">Jan 1, 1980</text>
    <text x="-46.7" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">1990</text>
    <text x="18.15" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2000</text>
    <text x="83.03" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2010</text>
    <text x="147.88" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2020</text>
    <text x="212.76" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2030</text>
    <text x="-111.58" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">Jan 1, 1980</text>
    <text x="-46.7" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">1990</text>
    <text x="18.15" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2000</text>
    <text x="83.03" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2010</text>
    <text x="147.88" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2020</text>
    <text x="212.76" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2030</text>
    <text x="-111.58" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">Jan 1, 1980</text>
    <text x="-46.7" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">1990</text>
    <text x="18.15" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2000</text>
    <text x="83.03" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2010</text>
    <text x="147.88" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2020</text>
    <text x="212.76" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2030</text>
  </g>
  <g class="guide colorkey" id="img-2f634ffe-4">
    <g fill="#4C404B" font-size="2.82" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" id="img-2f634ffe-5">
      <text x="121.27" y="41.04" dy="0.35em" id="img-2f634ffe-6" class="color_Emission">Emission</text>
      <text x="121.27" y="44.67" dy="0.35em" id="img-2f634ffe-7" class="color_SoilMoisture">SoilMoisture</text>
      <text x="121.27" y="48.3" dy="0.35em" id="img-2f634ffe-8" class="color_t2m">t2m</text>
    </g>
    <g stroke="#000000" stroke-opacity="0.000" id="img-2f634ffe-9">
      <rect x="118.45" y="40.14" width="1.81" height="1.81" id="img-2f634ffe-10" class="color_Emission" fill="#00BFFF"/>
      <rect x="118.45" y="43.76" width="1.81" height="1.81" id="img-2f634ffe-11" class="color_SoilMoisture" fill="#D4CA3A"/>
      <rect x="118.45" y="47.39" width="1.81" height="1.81" id="img-2f634ffe-12" class="color_t2m" fill="#FF6DAE"/>
    </g>
    <g fill="#362A35" font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" stroke="#000000" stroke-opacity="0.000" id="img-2f634ffe-13">
      <text x="118.45" y="37.22" id="img-2f634ffe-14">Color</text>
    </g>
  </g>
<g clip-path="url(#img-2f634ffe-15)">
  <g id="img-2f634ffe-16">
    <g pointer-events="visible" opacity="1" fill="#000000" fill-opacity="0.000" stroke="#000000" stroke-opacity="0.000" class="guide background" id="img-2f634ffe-17">
      <rect x="16.15" y="5" width="101.3" height="75.72" id="img-2f634ffe-18"/>
    </g>
    <g class="guide ygridlines xfixed" stroke-dasharray="0.5,0.5" stroke-width="0.2" stroke="#D0D0E0" id="img-2f634ffe-19">
      <path fill="none" d="M16.15,168.36 L 117.45 168.36" id="img-2f634ffe-20" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,150.43 L 117.45 150.43" id="img-2f634ffe-21" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,132.5 L 117.45 132.5" id="img-2f634ffe-22" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,114.57 L 117.45 114.57" id="img-2f634ffe-23" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,96.64 L 117.45 96.64" id="img-2f634ffe-24" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,78.72 L 117.45 78.72" id="img-2f634ffe-25" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M16.15,60.79 L 117.45 60.79" id="img-2f634ffe-26" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M16.15,42.86 L 117.45 42.86" id="img-2f634ffe-27" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M16.15,24.93 L 117.45 24.93" id="img-2f634ffe-28" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M16.15,7 L 117.45 7" id="img-2f634ffe-29" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M16.15,-10.93 L 117.45 -10.93" id="img-2f634ffe-30" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-28.86 L 117.45 -28.86" id="img-2f634ffe-31" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-46.79 L 117.45 -46.79" id="img-2f634ffe-32" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-64.72 L 117.45 -64.72" id="img-2f634ffe-33" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-82.64 L 117.45 -82.64" id="img-2f634ffe-34" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M16.15,150.43 L 117.45 150.43" id="img-2f634ffe-35" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,146.84 L 117.45 146.84" id="img-2f634ffe-36" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,143.26 L 117.45 143.26" id="img-2f634ffe-37" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,139.67 L 117.45 139.67" id="img-2f634ffe-38" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,136.09 L 117.45 136.09" id="img-2f634ffe-39" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,132.5 L 117.45 132.5" id="img-2f634ffe-40" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,128.92 L 117.45 128.92" id="img-2f634ffe-41" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,125.33 L 117.45 125.33" id="img-2f634ffe-42" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,121.74 L 117.45 121.74" id="img-2f634ffe-43" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,118.16 L 117.45 118.16" id="img-2f634ffe-44" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,114.57 L 117.45 114.57" id="img-2f634ffe-45" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,110.99 L 117.45 110.99" id="img-2f634ffe-46" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,107.4 L 117.45 107.4" id="img-2f634ffe-47" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,103.82 L 117.45 103.82" id="img-2f634ffe-48" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,100.23 L 117.45 100.23" id="img-2f634ffe-49" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,96.64 L 117.45 96.64" id="img-2f634ffe-50" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,93.06 L 117.45 93.06" id="img-2f634ffe-51" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,89.47 L 117.45 89.47" id="img-2f634ffe-52" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,85.89 L 117.45 85.89" id="img-2f634ffe-53" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,82.3 L 117.45 82.3" id="img-2f634ffe-54" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,78.72 L 117.45 78.72" id="img-2f634ffe-55" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,75.13 L 117.45 75.13" id="img-2f634ffe-56" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,71.54 L 117.45 71.54" id="img-2f634ffe-57" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,67.96 L 117.45 67.96" id="img-2f634ffe-58" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,64.37 L 117.45 64.37" id="img-2f634ffe-59" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,60.79 L 117.45 60.79" id="img-2f634ffe-60" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,57.2 L 117.45 57.2" id="img-2f634ffe-61" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,53.61 L 117.45 53.61" id="img-2f634ffe-62" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,50.03 L 117.45 50.03" id="img-2f634ffe-63" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,46.44 L 117.45 46.44" id="img-2f634ffe-64" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,42.86 L 117.45 42.86" id="img-2f634ffe-65" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,39.27 L 117.45 39.27" id="img-2f634ffe-66" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,35.69 L 117.45 35.69" id="img-2f634ffe-67" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,32.1 L 117.45 32.1" id="img-2f634ffe-68" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,28.51 L 117.45 28.51" id="img-2f634ffe-69" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,24.93 L 117.45 24.93" id="img-2f634ffe-70" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,21.34 L 117.45 21.34" id="img-2f634ffe-71" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,17.76 L 117.45 17.76" id="img-2f634ffe-72" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,14.17 L 117.45 14.17" id="img-2f634ffe-73" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,10.59 L 117.45 10.59" id="img-2f634ffe-74" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,7 L 117.45 7" id="img-2f634ffe-75" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,3.41 L 117.45 3.41" id="img-2f634ffe-76" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-0.17 L 117.45 -0.17" id="img-2f634ffe-77" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-3.76 L 117.45 -3.76" id="img-2f634ffe-78" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-7.34 L 117.45 -7.34" id="img-2f634ffe-79" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-10.93 L 117.45 -10.93" id="img-2f634ffe-80" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-14.51 L 117.45 -14.51" id="img-2f634ffe-81" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-18.1 L 117.45 -18.1" id="img-2f634ffe-82" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-21.69 L 117.45 -21.69" id="img-2f634ffe-83" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-25.27 L 117.45 -25.27" id="img-2f634ffe-84" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-28.86 L 117.45 -28.86" id="img-2f634ffe-85" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-32.44 L 117.45 -32.44" id="img-2f634ffe-86" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-36.03 L 117.45 -36.03" id="img-2f634ffe-87" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-39.61 L 117.45 -39.61" id="img-2f634ffe-88" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-43.2 L 117.45 -43.2" id="img-2f634ffe-89" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-46.79 L 117.45 -46.79" id="img-2f634ffe-90" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-50.37 L 117.45 -50.37" id="img-2f634ffe-91" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-53.96 L 117.45 -53.96" id="img-2f634ffe-92" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-57.54 L 117.45 -57.54" id="img-2f634ffe-93" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-61.13 L 117.45 -61.13" id="img-2f634ffe-94" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-64.72 L 117.45 -64.72" id="img-2f634ffe-95" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M16.15,204.22 L 117.45 204.22" id="img-2f634ffe-96" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M16.15,132.5 L 117.45 132.5" id="img-2f634ffe-97" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M16.15,60.79 L 117.45 60.79" id="img-2f634ffe-98" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M16.15,-10.93 L 117.45 -10.93" id="img-2f634ffe-99" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M16.15,-82.64 L 117.45 -82.64" id="img-2f634ffe-100" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M16.15,150.43 L 117.45 150.43" id="img-2f634ffe-101" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,132.5 L 117.45 132.5" id="img-2f634ffe-102" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,114.57 L 117.45 114.57" id="img-2f634ffe-103" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,96.64 L 117.45 96.64" id="img-2f634ffe-104" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,78.72 L 117.45 78.72" id="img-2f634ffe-105" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,60.79 L 117.45 60.79" id="img-2f634ffe-106" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,42.86 L 117.45 42.86" id="img-2f634ffe-107" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,24.93 L 117.45 24.93" id="img-2f634ffe-108" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,7 L 117.45 7" id="img-2f634ffe-109" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-10.93 L 117.45 -10.93" id="img-2f634ffe-110" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-28.86 L 117.45 -28.86" id="img-2f634ffe-111" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-46.79 L 117.45 -46.79" id="img-2f634ffe-112" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M16.15,-64.72 L 117.45 -64.72" id="img-2f634ffe-113" gadfly:scale="5.0" visibility="hidden"/>
    </g>
    <g class="guide xgridlines yfixed" stroke-dasharray="0.5,0.5" stroke-width="0.2" stroke="#D0D0E0" id="img-2f634ffe-114">
      <path fill="none" d="M-111.58,5 L -111.58 80.72" id="img-2f634ffe-115" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-79.13,5 L -79.13 80.72" id="img-2f634ffe-116" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-46.7,5 L -46.7 80.72" id="img-2f634ffe-117" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-14.28,5 L -14.28 80.72" id="img-2f634ffe-118" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M18.15,5 L 18.15 80.72" id="img-2f634ffe-119" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M50.6,5 L 50.6 80.72" id="img-2f634ffe-120" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M83.03,5 L 83.03 80.72" id="img-2f634ffe-121" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M115.45,5 L 115.45 80.72" id="img-2f634ffe-122" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M147.88,5 L 147.88 80.72" id="img-2f634ffe-123" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M180.33,5 L 180.33 80.72" id="img-2f634ffe-124" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M212.76,5 L 212.76 80.72" id="img-2f634ffe-125" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M245.19,5 L 245.19 80.72" id="img-2f634ffe-126" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-111.58,5 L -111.58 80.72" id="img-2f634ffe-127" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M-46.7,5 L -46.7 80.72" id="img-2f634ffe-128" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M18.15,5 L 18.15 80.72" id="img-2f634ffe-129" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M83.03,5 L 83.03 80.72" id="img-2f634ffe-130" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M147.88,5 L 147.88 80.72" id="img-2f634ffe-131" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M212.76,5 L 212.76 80.72" id="img-2f634ffe-132" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M-111.58,5 L -111.58 80.72" id="img-2f634ffe-133" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M-46.7,5 L -46.7 80.72" id="img-2f634ffe-134" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M18.15,5 L 18.15 80.72" id="img-2f634ffe-135" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M83.03,5 L 83.03 80.72" id="img-2f634ffe-136" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M147.88,5 L 147.88 80.72" id="img-2f634ffe-137" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M212.76,5 L 212.76 80.72" id="img-2f634ffe-138" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M-111.58,5 L -111.58 80.72" id="img-2f634ffe-139" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M-46.7,5 L -46.7 80.72" id="img-2f634ffe-140" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M18.15,5 L 18.15 80.72" id="img-2f634ffe-141" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M83.03,5 L 83.03 80.72" id="img-2f634ffe-142" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M147.88,5 L 147.88 80.72" id="img-2f634ffe-143" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M212.76,5 L 212.76 80.72" id="img-2f634ffe-144" gadfly:scale="9.131666666666666" visibility="hidden"/>
    </g>
    <g class="plotpanel" id="img-2f634ffe-145">
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry color_t2m" stroke-dasharray="none" stroke="#FF6DAE" id="img-2f634ffe-146">
        <path fill="none" d="M18.15,52.38 L 18.29 50.82 18.44 50.01 18.58 50.34 18.72 51.26 18.86 49.37 19 49.89 19.15 50.8 19.29 52.7 19.43 55.68 19.57 55.32 19.72 52.04 19.86 49.29 20 47.16 20.14 45.9 20.28 44.58 20.43 42.38 20.57 38.79 20.71 37.37 20.85 34.36 20.99 33.34 21.14 30.79 21.28 27.22 21.42 24.29 21.56 22.18 21.7 21.07 21.85 19.11 21.99 16.8 22.13 16.22 22.27 17.78 22.41 18.02 22.56 18.87 22.7 20.62 22.84 23.37 22.98 27.02 23.13 30.17 23.27 33.35 23.41 37.04 23.55 40.4 23.69 45.02 23.84 45.11 23.98 44.73 24.12 44.39 24.26 51.04 24.4 50.96 24.55 50.4 24.65 49.42 24.79 50.44 24.94 47.4 25.08 49.86 25.22 48.15 25.36 46.01 25.5 46.45 25.65 48.72 25.79 43.93 25.93 51.85 26.07 57.7 26.22 49.64 26.36 47.12 26.5 44.03 26.64 44.62 26.78 49.32 26.93 43.85 27.07 39.58 27.21 37.52 27.35 32.16 27.49 29.28 27.64 29.16 27.78 24.52 27.92 22.25 28.06 18.91 28.2 19.29 28.35 16.94 28.49 15.59 28.63 12.09 28.77 17.88 28.91 19.19 29.06 21.36 29.2 20.82 29.34 26.93 29.48 28.24 29.62 33.62 29.77 33.5 29.91 43.18 30.05 47.79 30.19 50.01 30.34 40.9 30.48 43.93 30.62 46.52 30.76 59.23 30.9 56.18 31.05 46.07 31.13 47.89 31.28 48.56 31.42 53.2 31.56 50.6 31.7 54.49 31.84 57.68 31.99 55.41 32.13 50.58 32.27 50.63 32.41 46.65 32.56 49.31 32.7 51.68 32.84 51.31 32.98 46.37 33.12 43.24 33.27 41.1 33.41 40.69 33.55 36.83 33.69 36.22 33.83 33.99 33.98 35.71 34.12 29.88 34.26 27.83 34.4 25.2 34.54 23.51 34.69 20 34.83 19.08 34.97 17.41 35.11 15.33 35.25 19.04 35.4 17.68 35.54 19.18 35.68 24.15 35.82 21.85 35.96 23.08 36.11 25.9 36.25 26.46 36.39 30.77 36.53 30.46 36.68 35.36 36.82 39.61 36.96 38.49 37.1 39.53 37.24 49.9 37.39 50.32 37.53 49.08 37.62 54.12 37.76 44.65 37.9 43.74 38.04 46.61 38.18 42.99 38.33 45.77 38.47 44.55 38.61 43.14 38.75 50.12 38.9 62.15 39.04 50.89 39.18 45.76 39.32 47.53 39.46 44.46 39.61 42.48 39.75 39.82 39.89 41.67 40.03 35.57 40.17 35.8 40.32 33.07 40.46 31.49 40.6 30.49 40.74 27.17 40.88 23.49 41.03 21.86 41.17 19.28 41.31 18.03 41.45 13.33 41.59 16.79 41.74 17.46 41.88 17.42 42.02 16.32 42.16 19.96 42.3 21.15 42.45 22.88 42.59 30.29 42.73 37.31 42.87 33.15 43.02 30.24 43.16 41.04 43.3 39.87 43.44 48.91 43.58 47.73 43.73 48.47 43.87 52.73 44.01 44.68 44.1 50.71 44.24 57.67 44.38 50.41 44.52 47.01 44.67 48.11 44.81 48.01 44.95 49.58 45.09 47.42 45.24 51.32 45.38 53.95 45.52 50.05 45.66 60.49 45.8 46.66 45.95 44.93 46.09 43.75 46.23 42.11 46.37 40.32 46.51 36.21 46.66 35.52 46.8 33.62 46.94 34.05 47.08 30.11 47.22 26.34 47.37 22.19 47.51 19.22 47.65 15.92 47.79 16.06 47.93 12.74 48.08 11.83 48.22 11.73 48.36 14.32 48.5 14.75 48.64 15.51 48.79 20.67 48.93 23.53 49.07 26.49 49.21 28.36 49.36 33.26 49.5 42.97 49.64 40.05 49.78 39.77 49.92 38.5 50.07 45.87 50.21 45.88 50.35 46.65 50.49 46.38 50.6 44.52 50.74 43.82 50.88 43.64 51.02 43.95 51.17 48.43 51.31 47.08 51.45 44.94 51.59 47.09 51.73 43.63 51.88 44.38 52.02 49.43 52.16 57.45 52.3 49.19 52.45 42.63 52.59 40.09 52.73 39.2 52.87 36.04 53.01 35.81 53.16 34.71 53.3 30.74 53.44 27.39 53.58 26.89 53.72 22.68 53.87 19.73 54.01 17.59 54.15 17.85 54.29 13.85 54.43 12.4 54.58 11.24 54.72 13.22 54.86 15.97 55 15.68 55.14 18.17 55.29 20.51 55.43 28.1 55.57 30.37 55.71 31.92 55.86 40.74 56 37.98 56.14 50.62 56.28 52.9 56.42 47.8 56.57 39.52 56.71 44.78 56.85 44.87 56.99 46.42 57.08 48.12 57.22 50.61 57.36 56.19 57.51 66.33 57.65 57.46 57.79 46.62 57.93 48.23 58.07 54.79 58.22 50.47 58.36 50.06 58.5 52.4 58.64 46.47 58.79 47.49 58.93 52.45 59.07 52.23 59.21 46.01 59.35 43.5 59.5 39.8 59.64 39.38 59.78 33.09 59.92 33.51 60.06 30.8 60.21 27.33 60.35 25.03 60.49 23.96 60.63 23.39 60.77 21.49 60.92 21.35 61.06 20.05 61.2 19.06 61.34 18.72 61.48 20.62 61.63 22.1 61.77 25.95 61.91 24.96 62.05 27.6 62.2 31.39 62.34 37.6 62.48 43.84 62.62 51.57 62.76 52.51 62.91 43.7 63.05 45.57 63.19 54 63.33 57.87 63.47 54.79 63.56 61.01 63.7 49.68 63.85 54.86 63.99 44.96 64.13 46.94 64.27 48.21 64.41 53.27 64.56 57.01 64.7 74.32 64.84 63.02 64.98 62.19 65.13 50.58 65.27 49.85 65.41 49.19 65.55 47.76 65.69 45.23 65.84 44.13 65.98 40.26 66.12 40.62 66.26 36.44 66.4 34.69 66.55 30.13 66.69 26.66 66.83 22.01 66.97 23.61 67.11 20.41 67.26 19.13 67.4 15.6 67.54 14.75 67.68 16 67.82 15.7 67.97 16.49 68.11 18.64 68.25 22.52 68.39 27.81 68.54 33.85 68.68 40.02 68.82 35.08 68.96 39.53 69.1 46.49 69.25 45.48 69.39 39.41 69.53 44.16 69.67 52.84 69.81 53.06 69.96 50.77 70.04 64.97 70.19 65.39 70.33 47.93 70.47 50.06 70.61 52.63 70.75 53.63 70.9 55.58 71.04 51.87 71.18 51.71 71.32 57.83 71.47 70.68 71.61 48.99 71.75 51.65 71.89 48.21 72.03 50.67 72.18 51.62 72.32 45.63 72.46 42.07 72.6 39.07 72.74 37.89 72.89 37.15 73.03 32.75 73.17 29.35 73.31 28.15 73.45 23.31 73.6 24.03 73.74 20.2 73.88 16.42 74.02 17.59 74.16 20.31 74.31 21.85 74.45 23.17 74.59 25.1 74.73 25.17 74.88 31.36 75.02 31.67 75.16 37.72 75.3 40.43 75.44 43.95 75.59 45.85 75.73 45.61 75.87 51.03 76.01 43.03 76.15 46.39 76.3 45.91 76.44 65.86 76.54 58.1 76.69 48.09 76.83 48.04 76.97 55.28 77.11 56.65 77.25 50.84 77.4 50.53 77.54 48.9 77.68 49.62 77.82 71.05 77.97 57.41 78.11 52.58 78.25 52.17 78.39 49.86 78.53 43.65 78.68 43.21 78.82 43.47 78.96 38.34 79.1 35.4 79.24 35.11 79.39 34.32 79.53 32.7 79.67 28.42 79.81 23.98 79.95 21.35 80.1 23.56 80.24 22.82 80.38 21.86 80.52 21.24 80.66 22.25 80.81 21.34 80.95 21.6 81.09 21.94 81.23 25.93 81.37 26.21 81.52 27.01 81.66 32.45 81.8 40.06 81.94 42.81 82.09 46.64 82.23 53.78 82.37 50.8 82.51 42.85 82.65 54.41 82.8 44.37 82.94 45.5 83.03 44.99 83.17 49.3 83.31 54.69 83.45 48.74 83.59 56.71 83.74 49.89 83.88 50.34 84.02 58.54 84.16 61.29 84.31 55.87 84.45 53.12 84.59 56.8 84.73 49.89 84.87 49.47 85.02 50.55 85.16 48.22 85.3 44.5 85.44 43.43 85.58 39.48 85.73 37.51 85.87 35.78 86.01 35.01 86.15 31.92 86.29 30.9 86.44 28.44 86.58 27.01 86.72 23.51 86.86 21.34 87 21.32 87.15 20.89 87.29 18.04 87.43 19.5 87.57 19.81 87.71 23.04 87.86 34.04 88 34.88 88.14 34.39 88.28 36.15 88.43 44.41 88.57 42.55 88.71 40.68 88.85 44.7 88.99 49.15 89.14 54.54 89.28 57.6 89.42 54.42" id="img-2f634ffe-147"/>
      </g>
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry color_SoilMoisture" stroke-dasharray="none" stroke="#D4CA3A" id="img-2f634ffe-148">
        <path fill="none" d="M18.15,59.6 L 18.29 59.9 18.44 59.86 18.58 60.05 18.72 60.02 18.86 60.2 19 60.16 19.15 60.06 19.29 60.19 19.43 59.99 19.57 59.91 19.72 59.37 19.86 59.47 20 60.07 20.14 60.79 20.28 60.03 20.43 60.3 20.57 60.36 20.71 60.14 20.85 60.52 20.99 60.31 21.14 60.33 21.28 60.28 21.42 60.1 21.56 60.42 21.7 60.22 21.85 60.2 21.99 60.15 22.13 60.34 22.27 59.99 22.41 60.14 22.56 60.18 22.7 59.37 22.84 60.2 22.98 60.37 23.13 60.4 23.27 60.18 23.41 59.89 23.55 59.8 23.69 59.94 23.84 59.79 23.98 59.85 24.12 59.84 24.26 59.97 24.4 60.05 24.55 60.08 24.65 59.6 24.79 59.9 24.94 59.86 25.08 60.05 25.22 60.02 25.36 60.2 25.5 60.16 25.65 60.06 25.79 60.19 25.93 59.99 26.07 59.91 26.22 60.11 26.36 60.79 26.5 59.94 26.64 60.25 26.78 60.1 26.93 60.33 27.07 60.16 27.21 60.11 27.35 60.32 27.49 60.34 27.64 60.27 27.78 60.17 27.92 59.84 28.06 60.12 28.2 60.02 28.35 59.93 28.49 59.77 28.63 59.99 28.77 60.04 28.91 60.11 29.06 60.06 29.2 59.94 29.34 60.28 29.48 59.94 29.62 60.79 29.77 60.09 29.91 60.04 30.05 60.05 30.19 60.79 30.34 59.79 30.48 59.85 30.62 59.84 30.76 59.97 30.9 60.05 31.05 60.08 31.13 59.6 31.28 59.9 31.42 59.86 31.56 60.05 31.7 60.02 31.84 60.2 31.99 60.16 32.13 60.06 32.27 60.19 32.41 59.99 32.56 59.91 32.7 60.22 32.84 60.19 32.98 59.91 33.12 60.09 33.27 60.25 33.41 60.42 33.55 59.96 33.69 59.75 33.83 60.17 33.98 60.23 34.12 60.46 34.26 60.41 34.4 59.98 34.54 60.33 34.69 60.33 34.83 60.43 34.97 60.38 35.11 60.26 35.25 60.49 35.4 60.46 35.54 60.43 35.68 60.46 35.82 60.28 35.96 60.31 36.11 59.98 36.25 59.89 36.39 59.94 36.53 60.01 36.68 59.96 36.82 59.86 36.96 59.99 37.1 60.35 37.24 60.5 37.39 60.53 37.53 60.37 37.62 59.6 37.76 59.99 37.9 60.03 38.04 59.83 38.18 60.37 38.33 60.34 38.47 60.32 38.61 60.06 38.75 60.41 38.9 60.02 39.04 60.15 39.18 60.07 39.32 60.2 39.46 60.13 39.61 60.23 39.75 60.27 39.89 60.24 40.03 60.23 40.17 60.42 40.32 60.55 40.46 60.62 40.6 60.31 40.74 60.2 40.88 59.94 41.03 60.17 41.17 60.18 41.31 60.41 41.45 60.47 41.59 60.46 41.74 60.26 41.88 60.19 42.02 60.28 42.16 60.39 42.3 60.37 42.45 59.98 42.59 60.03 42.73 60.08 42.87 60.09 43.02 59.83 43.16 59.93 43.3 59.77 43.44 59.86 43.58 59.84 43.73 60.05 43.87 59.97 44.01 60.05 44.1 59.53 44.24 60.07 44.38 59.72 44.52 59.74 44.67 59.7 44.81 60.05 44.95 60.15 45.09 60.06 45.24 60.42 45.38 59.79 45.52 59.81 45.66 59.95 45.8 60.03 45.95 60.06 46.09 60.18 46.23 60.2 46.37 60.16 46.51 60.18 46.66 60.24 46.8 60.4 46.94 60.35 47.08 60.25 47.22 60.22 47.37 60.17 47.51 59.99 47.65 59.42 47.79 59.97 47.93 60.31 48.08 60.31 48.22 60.3 48.36 60.32 48.5 60.37 48.64 60.36 48.79 60.26 48.93 60.16 49.07 60.12 49.21 60.03 49.36 60.06 49.5 60.02 49.64 59.77 49.78 59.95 49.92 60.04 50.07 59.74 50.21 59.82 50.35 60.09 50.49 59.78 50.6 59.45 50.74 59.74 50.88 60.06 51.02 60.14 51.17 60.42 51.31 60.47 51.45 60.37 51.59 60.06 51.73 60.73 51.88 60.29 52.02 59.67 52.16 60 52.3 59.99 52.45 60.17 52.59 60.2 52.73 60.18 52.87 60.01 53.01 59.95 53.16 60.31 53.3 60.21 53.44 60.23 53.58 60.17 53.72 59.79 53.87 59.65 54.01 60.41 54.15 60.28 54.29 60.14 54.43 60.16 54.58 60.33 54.72 60.41 54.86 60.51 55 60.39 55.14 60.05 55.29 60.25 55.43 60 55.57 60.2 55.71 60.2 55.86 60.06 56 60.09 56.14 59.71 56.28 59.98 56.42 60.05 56.57 59.7 56.71 59.67 56.85 60.12 56.99 60.34 57.08 60.13 57.22 60.2 57.36 60.2 57.51 60.34 57.65 60.57 57.79 60.79 57.93 60.38 58.07 60.06 58.22 60.19 58.36 60.38 58.5 60.29 58.64 59.84 58.79 60.02 58.93 60.07 59.07 60.17 59.21 60.38 59.35 60.29 59.5 60.11 59.64 60.1 59.78 60.15 59.92 60.31 60.06 60.09 60.21 59.95 60.35 59.94 60.49 60.19 60.63 60.43 60.77 60.44 60.92 60.18 61.06 60.12 61.2 60.17 61.34 60.07 61.48 60.16 61.63 60.25 61.77 60.33 61.91 60.07 62.05 60.16 62.2 60.06 62.34 60.12 62.48 60.06 62.62 59.78 62.76 59.85 62.91 59.75 63.05 59.88 63.19 59.8 63.33 59.75 63.47 59.85 63.56 59.31 63.7 59.5 63.85 59.72 63.99 59.91 64.13 59.89 64.27 60.2 64.41 60.16 64.56 59.96 64.7 59.75 64.84 59.62 64.98 59.76 65.13 60.34 65.27 59.96 65.41 60.09 65.55 60.35 65.69 60.41 65.84 60.1 65.98 60.36 66.12 60.44 66.26 60.33 66.4 60.42 66.55 60.17 66.69 60.2 66.83 60.09 66.97 60.2 67.11 60.37 67.26 60.16 67.4 60.15 67.54 60.26 67.68 60.24 67.82 60.17 67.97 60.2 68.11 60.23 68.25 60.19 68.39 60.24 68.54 60.1 68.68 60.13 68.82 60.36 68.96 60.04 69.1 59.74 69.25 59.79 69.39 59.67 69.53 59.78 69.67 59.97 69.81 60.05 69.96 60.08 70.04 59.6 70.19 59.9 70.33 59.45 70.47 60.59 70.61 59.89 70.75 59.71 70.9 59.72 71.04 59.89 71.18 59.78 71.32 59.88 71.47 60.05 71.61 59.81 71.75 60 71.89 59.87 72.03 60.09 72.18 60.08 72.32 60.16 72.46 60.21 72.6 60.47 72.74 60.48 72.89 60.46 73.03 60.39 73.17 60.26 73.31 60.2 73.45 60.25 73.6 60.36 73.74 60.44 73.88 60.27 74.02 60.09 74.16 60.25 74.31 60.23 74.45 60.12 74.59 60.26 74.73 60.08 74.88 60.16 75.02 60.26 75.16 60.15 75.3 60.12 75.44 60.19 75.59 60.22 75.73 59.68 75.87 59.81 76.01 59.63 76.15 59.62 76.3 59.87 76.44 60.08 76.54 59.6 76.69 59.9 76.83 59.83 76.97 59.8 77.11 59.31 77.25 59.87 77.4 60.16 77.54 60.06 77.68 59.8 77.82 59.8 77.97 59.81 78.11 59.92 78.25 60.13 78.39 60.23 78.53 60.4 78.68 60.4 78.82 60.45 78.96 60.34 79.1 60.31 79.24 60.25 79.39 60.33 79.53 60.27 79.67 60.09 79.81 60.31 79.95 60.2 80.1 60.08 80.24 60.4 80.38 60.4 80.52 60.16 80.66 60.28 80.81 60.11 80.95 60.1 81.09 60.31 81.23 60.32 81.37 60.13 81.52 60.14 81.66 59.99 81.8 59.89 81.94 60.17 82.09 59.78 82.23 59.77 82.37 59.99 82.51 59.81 82.65 60.32 82.8 60.05 82.94 60.08 83.03 59.6 83.17 59.9 83.31 59.86 83.45 60.05 83.59 60.02 83.74 60.2 83.88 60.04 84.02 60.34 84.16 60.4 84.31 60.19 84.45 59.71 84.59 59.61 84.73 59.78 84.87 60.14 85.02 59.98 85.16 59.98 85.3 60 85.44 59.92 85.58 60.16 85.73 60.15 85.87 60.1 86.01 60.29 86.15 60.35 86.29 60.43 86.44 60.4 86.58 60.37 86.72 60.13 86.86 60.15 87 60.3 87.15 60.16 87.29 60.01 87.43 59.97 87.57 59.97 87.71 60.15 87.86 59.99 88 60.22 88.14 60.16 88.28 59.96 88.43 59.79 88.57 59.72 88.71 59.46 88.85 59.46 88.99 59.84 89.14 59.97 89.28 60.05 89.42 60.08" id="img-2f634ffe-149"/>
      </g>
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry color_Emission" stroke-dasharray="none" stroke="#00BFFF" id="img-2f634ffe-150">
        <path fill="none" d="M18.15,60.79 L 18.29 60.79 18.44 60.79 18.58 60.79 18.72 60.79 18.86 60.79 19 60.79 19.15 60.79 19.29 60.79 19.43 60.79 19.57 60.79 19.72 60.74 19.86 60.66 20 60.66 20.14 60.66 20.28 60.79 20.43 60.79 20.57 60.79 20.71 60.79 20.85 60.79 20.99 60.79 21.14 60.79 21.28 60.79 21.42 60.79 21.56 60.79 21.7 60.79 21.85 60.79 21.99 60.79 22.13 60.79 22.27 60.79 22.41 60.79 22.56 60.79 22.7 60.79 22.84 60.79 22.98 60.79 23.13 60.79 23.27 60.79 23.41 60.79 23.55 60.79 23.69 60.79 23.84 60.79 23.98 60.79 24.12 60.79 24.26 60.79 24.4 60.79 24.55 60.79 24.65 60.79 24.79 60.79 24.94 60.79 25.08 60.79 25.22 60.79 25.36 60.79 25.5 60.79 25.65 60.79 25.79 60.79 25.93 60.79 26.07 60.79 26.22 60.79 26.36 60.79 26.5 60.79 26.64 60.79 26.78 60.79 26.93 60.79 27.07 60.79 27.21 60.79 27.35 60.79 27.49 60.79 27.64 60.79 27.78 60.79 27.92 60.79 28.06 60.79 28.2 60.79 28.35 60.79 28.49 60.79 28.63 60.79 28.77 60.79 28.91 60.79 29.06 60.79 29.2 60.79 29.34 60.79 29.48 60.79 29.62 60.79 29.77 60.79 29.91 60.79 30.05 60.79 30.19 60.79 30.34 60.79 30.48 60.79 30.62 60.79 30.76 60.79 30.9 60.79 31.05 60.79 31.13 60.79 31.28 60.79 31.42 60.79 31.56 60.79 31.7 60.79 31.84 60.79 31.99 60.79 32.13 60.79 32.27 60.79 32.41 60.79 32.56 60.79 32.7 60.79 32.84 60.79 32.98 60.79 33.12 60.79 33.27 60.79 33.41 60.79 33.55 60.79 33.69 60.79 33.83 60.79 33.98 60.79 34.12 60.79 34.26 60.79 34.4 60.79 34.54 60.79 34.69 60.79 34.83 60.79 34.97 60.79 35.11 60.79 35.25 60.79 35.4 60.79 35.54 60.79 35.68 60.79 35.82 60.79 35.96 60.79 36.11 60.79 36.25 60.79 36.39 60.79 36.53 60.79 36.68 60.79 36.82 60.79 36.96 60.79 37.1 60.79 37.24 60.79 37.39 60.79 37.53 60.79 37.62 60.79 37.76 60.79 37.9 60.79 38.04 60.79 38.18 60.79 38.33 60.79 38.47 60.79 38.61 60.79 38.75 60.79 38.9 60.79 39.04 60.79 39.18 60.79 39.32 60.79 39.46 60.79 39.61 60.79 39.75 60.79 39.89 60.79 40.03 60.79 40.17 60.79 40.32 60.79 40.46 60.79 40.6 60.79 40.74 60.79 40.88 60.79 41.03 60.79 41.17 60.79 41.31 60.79 41.45 60.79 41.59 60.79 41.74 60.79 41.88 60.79 42.02 60.79 42.16 60.79 42.3 60.79 42.45 60.79 42.59 60.79 42.73 60.79 42.87 60.79 43.02 60.79 43.16 60.79 43.3 60.79 43.44 60.79 43.58 60.79 43.73 60.79 43.87 60.79 44.01 60.79 44.1 60.79 44.24 60.79 44.38 60.79 44.52 60.79 44.67 60.79 44.81 60.79 44.95 60.79 45.09 60.79 45.24 60.79 45.38 60.79 45.52 60.79 45.66 60.79 45.8 60.79 45.95 60.79 46.09 60.79 46.23 60.79 46.37 60.79 46.51 60.79 46.66 60.79 46.8 60.79 46.94 60.79 47.08 60.79 47.22 60.79 47.37 60.79 47.51 60.79 47.65 60.79 47.79 60.79 47.93 60.79 48.08 60.79 48.22 60.79 48.36 60.79 48.5 60.79 48.64 60.79 48.79 60.79 48.93 60.79 49.07 60.79 49.21 60.79 49.36 60.79 49.5 60.79 49.64 60.79 49.78 60.79 49.92 60.79 50.07 60.79 50.21 60.79 50.35 60.79 50.49 60.79 50.6 60.79 50.74 60.79 50.88 60.79 51.02 60.79 51.17 60.79 51.31 60.79 51.45 60.79 51.59 60.79 51.73 60.79 51.88 60.79 52.02 60.79 52.16 60.79 52.3 60.79 52.45 60.79 52.59 60.79 52.73 60.79 52.87 60.79 53.01 60.79 53.16 60.79 53.3 60.79 53.44 60.79 53.58 60.79 53.72 60.79 53.87 60.79 54.01 60.79 54.15 60.79 54.29 60.79 54.43 60.79 54.58 60.79 54.72 60.79 54.86 60.79 55 60.79 55.14 60.79 55.29 60.79 55.43 60.79 55.57 60.79 55.71 60.79 55.86 60.79 56 60.79 56.14 60.79 56.28 60.79 56.42 60.79 56.57 60.79 56.71 60.79 56.85 60.79 56.99 60.79 57.08 60.79 57.22 60.79 57.36 60.79 57.51 60.79 57.65 60.79 57.79 60.79 57.93 60.79 58.07 60.79 58.22 60.79 58.36 60.79 58.5 60.79 58.64 60.3 58.79 59.48 58.93 59.48 59.07 59.48 59.21 60.79 59.35 60.79 59.5 60.79 59.64 60.79 59.78 60.79 59.92 60.79 60.06 60.79 60.21 60.79 60.35 60.79 60.49 60.79 60.63 60.79 60.77 60.79 60.92 60.79 61.06 60.79 61.2 60.79 61.34 60.79 61.48 60.79 61.63 60.79 61.77 60.79 61.91 60.79 62.05 60.79 62.2 60.79 62.34 60.79 62.48 60.79 62.62 60.79 62.76 60.79 62.91 60.79 63.05 60.79 63.19 60.79 63.33 60.79 63.47 60.79 63.56 60.79 63.7 60.79 63.85 60.79 63.99 60.79 64.13 60.79 64.27 60.79 64.41 60.79 64.56 60.79 64.7 60.79 64.84 60.79 64.98 60.79 65.13 60.79 65.27 60.79 65.41 60.79 65.55 60.79 65.69 60.79 65.84 60.79 65.98 60.79 66.12 60.79 66.26 60.79 66.4 60.79 66.55 60.79 66.69 60.79 66.83 60.79 66.97 60.79 67.11 60.79 67.26 60.79 67.4 60.79 67.54 60.79 67.68 60.79 67.82 60.79 67.97 60.79 68.11 60.79 68.25 60.79 68.39 60.79 68.54 60.79 68.68 60.79 68.82 60.79 68.96 60.79 69.1 60.79 69.25 60.79 69.39 60.79 69.53 60.79 69.67 60.79 69.81 60.79 69.96 60.79 70.04 60.79 70.19 60.79 70.33 60.79 70.47 60.79 70.61 60.79 70.75 60.79 70.9 60.79 71.04 60.79 71.18 60.79 71.32 60.79 71.47 60.79 71.61 60.79 71.75 60.79 71.89 60.79 72.03 60.79 72.18 60.79 72.32 60.79 72.46 60.79 72.6 60.79 72.74 60.79 72.89 60.79 73.03 60.79 73.17 60.79 73.31 60.79 73.45 60.79 73.6 60.79 73.74 60.79 73.88 60.79 74.02 60.79 74.16 60.79 74.31 60.79 74.45 60.79 74.59 60.79 74.73 60.79 74.88 60.79 75.02 60.79 75.16 60.79 75.3 60.79 75.44 60.79 75.59 60.79 75.73 60.79 75.87 60.79 76.01 60.79 76.15 60.79 76.3 60.79 76.44 60.79 76.54 60.79 76.69 60.79 76.83 60.79 76.97 60.79 77.11 60.79 77.25 60.79 77.4 60.79 77.54 60.79 77.68 60.79 77.82 60.79 77.97 60.79 78.11 60.79 78.25 60.79 78.39 60.79 78.53 60.79 78.68 60.79 78.82 60.79 78.96 60.79 79.1 60.79 79.24 60.79 79.39 60.79 79.53 60.79 79.67 60.79 79.81 60.79 79.95 60.79 80.1 60.79 80.24 60.79 80.38 60.79 80.52 60.79 80.66 60.79 80.81 60.79 80.95 60.79 81.09 60.79 81.23 60.79 81.37 60.79 81.52 60.79 81.66 60.79 81.8 60.79 81.94 60.79 82.09 60.79 82.23 60.79 82.37 60.79 82.51 60.79 82.65 60.79 82.8 60.79 82.94 60.79 83.03 60.79 83.17 60.79 83.31 60.79 83.45 60.79 83.59 60.79 83.74 60.79 83.88 60.79 84.02 60.79 84.16 60.79 84.31 60.79 84.45 60.79 84.59 60.79 84.73 60.79 84.87 60.79 85.02 60.79 85.16 60.79 85.3 60.79 85.44 60.79 85.58 60.79 85.73 60.79 85.87 60.79 86.01 60.79 86.15 60.79 86.29 60.79 86.44 60.79 86.58 60.79 86.72 60.79 86.86 60.79 87 60.79 87.15 60.79 87.29 60.79 87.43 60.79 87.57 60.79 87.71 60.79 87.86 60.79 88 60.79 88.14 60.79 88.28 60.79 88.43 60.79 88.57 60.79 88.71 60.79 88.85 60.79 88.99 60.79 89.14 60.79 89.28 60.79 89.42 60.79" id="img-2f634ffe-151"/>
      </g>
    </g>
    <g opacity="0" class="guide zoomslider" stroke="#000000" stroke-opacity="0.000" id="img-2f634ffe-152">
      <g fill="#EAEAEA" stroke-width="0.3" stroke-opacity="0" stroke="#6A6A6A" id="img-2f634ffe-153">
        <rect x="110.45" y="8" width="4" height="4" id="img-2f634ffe-154"/>
        <g class="button_logo" fill="#6A6A6A" id="img-2f634ffe-155">
          <path d="M111.25,9.6 L 112.05 9.6 112.05 8.8 112.85 8.8 112.85 9.6 113.65 9.6 113.65 10.4 112.85 10.4 112.85 11.2 112.05 11.2 112.05 10.4 111.25 10.4 z" id="img-2f634ffe-156"/>
        </g>
      </g>
      <g fill="#EAEAEA" id="img-2f634ffe-157">
        <rect x="90.95" y="8" width="19" height="4" id="img-2f634ffe-158"/>
      </g>
      <g class="zoomslider_thumb" fill="#6A6A6A" id="img-2f634ffe-159">
        <rect x="99.45" y="8" width="2" height="4" id="img-2f634ffe-160"/>
      </g>
      <g fill="#EAEAEA" stroke-width="0.3" stroke-opacity="0" stroke="#6A6A6A" id="img-2f634ffe-161">
        <rect x="86.45" y="8" width="4" height="4" id="img-2f634ffe-162"/>
        <g class="button_logo" fill="#6A6A6A" id="img-2f634ffe-163">
          <path d="M87.25,9.6 L 89.65 9.6 89.65 10.4 87.25 10.4 z" id="img-2f634ffe-164"/>
        </g>
      </g>
    </g>
  </g>
</g>
  <g class="guide ylabels" font-size="2.82" font-family="'PT Sans Caption','Helvetica Neue','Helvetica',sans-serif" fill="#6C606B" id="img-2f634ffe-165">
    <text x="15.15" y="168.36" text-anchor="end" dy="0.35em" id="img-2f634ffe-166" gadfly:scale="1.0" visibility="hidden">-30</text>
    <text x="15.15" y="150.43" text-anchor="end" dy="0.35em" id="img-2f634ffe-167" gadfly:scale="1.0" visibility="hidden">-25</text>
    <text x="15.15" y="132.5" text-anchor="end" dy="0.35em" id="img-2f634ffe-168" gadfly:scale="1.0" visibility="hidden">-20</text>
    <text x="15.15" y="114.57" text-anchor="end" dy="0.35em" id="img-2f634ffe-169" gadfly:scale="1.0" visibility="hidden">-15</text>
    <text x="15.15" y="96.64" text-anchor="end" dy="0.35em" id="img-2f634ffe-170" gadfly:scale="1.0" visibility="hidden">-10</text>
    <text x="15.15" y="78.72" text-anchor="end" dy="0.35em" id="img-2f634ffe-171" gadfly:scale="1.0" visibility="visible">-5</text>
    <text x="15.15" y="60.79" text-anchor="end" dy="0.35em" id="img-2f634ffe-172" gadfly:scale="1.0" visibility="visible">0</text>
    <text x="15.15" y="42.86" text-anchor="end" dy="0.35em" id="img-2f634ffe-173" gadfly:scale="1.0" visibility="visible">5</text>
    <text x="15.15" y="24.93" text-anchor="end" dy="0.35em" id="img-2f634ffe-174" gadfly:scale="1.0" visibility="visible">10</text>
    <text x="15.15" y="7" text-anchor="end" dy="0.35em" id="img-2f634ffe-175" gadfly:scale="1.0" visibility="visible">15</text>
    <text x="15.15" y="-10.93" text-anchor="end" dy="0.35em" id="img-2f634ffe-176" gadfly:scale="1.0" visibility="hidden">20</text>
    <text x="15.15" y="-28.86" text-anchor="end" dy="0.35em" id="img-2f634ffe-177" gadfly:scale="1.0" visibility="hidden">25</text>
    <text x="15.15" y="-46.79" text-anchor="end" dy="0.35em" id="img-2f634ffe-178" gadfly:scale="1.0" visibility="hidden">30</text>
    <text x="15.15" y="-64.72" text-anchor="end" dy="0.35em" id="img-2f634ffe-179" gadfly:scale="1.0" visibility="hidden">35</text>
    <text x="15.15" y="-82.64" text-anchor="end" dy="0.35em" id="img-2f634ffe-180" gadfly:scale="1.0" visibility="hidden">40</text>
    <text x="15.15" y="150.43" text-anchor="end" dy="0.35em" id="img-2f634ffe-181" gadfly:scale="10.0" visibility="hidden">-25</text>
    <text x="15.15" y="146.84" text-anchor="end" dy="0.35em" id="img-2f634ffe-182" gadfly:scale="10.0" visibility="hidden">-24</text>
    <text x="15.15" y="143.26" text-anchor="end" dy="0.35em" id="img-2f634ffe-183" gadfly:scale="10.0" visibility="hidden">-23</text>
    <text x="15.15" y="139.67" text-anchor="end" dy="0.35em" id="img-2f634ffe-184" gadfly:scale="10.0" visibility="hidden">-22</text>
    <text x="15.15" y="136.09" text-anchor="end" dy="0.35em" id="img-2f634ffe-185" gadfly:scale="10.0" visibility="hidden">-21</text>
    <text x="15.15" y="132.5" text-anchor="end" dy="0.35em" id="img-2f634ffe-186" gadfly:scale="10.0" visibility="hidden">-20</text>
    <text x="15.15" y="128.92" text-anchor="end" dy="0.35em" id="img-2f634ffe-187" gadfly:scale="10.0" visibility="hidden">-19</text>
    <text x="15.15" y="125.33" text-anchor="end" dy="0.35em" id="img-2f634ffe-188" gadfly:scale="10.0" visibility="hidden">-18</text>
    <text x="15.15" y="121.74" text-anchor="end" dy="0.35em" id="img-2f634ffe-189" gadfly:scale="10.0" visibility="hidden">-17</text>
    <text x="15.15" y="118.16" text-anchor="end" dy="0.35em" id="img-2f634ffe-190" gadfly:scale="10.0" visibility="hidden">-16</text>
    <text x="15.15" y="114.57" text-anchor="end" dy="0.35em" id="img-2f634ffe-191" gadfly:scale="10.0" visibility="hidden">-15</text>
    <text x="15.15" y="110.99" text-anchor="end" dy="0.35em" id="img-2f634ffe-192" gadfly:scale="10.0" visibility="hidden">-14</text>
    <text x="15.15" y="107.4" text-anchor="end" dy="0.35em" id="img-2f634ffe-193" gadfly:scale="10.0" visibility="hidden">-13</text>
    <text x="15.15" y="103.82" text-anchor="end" dy="0.35em" id="img-2f634ffe-194" gadfly:scale="10.0" visibility="hidden">-12</text>
    <text x="15.15" y="100.23" text-anchor="end" dy="0.35em" id="img-2f634ffe-195" gadfly:scale="10.0" visibility="hidden">-11</text>
    <text x="15.15" y="96.64" text-anchor="end" dy="0.35em" id="img-2f634ffe-196" gadfly:scale="10.0" visibility="hidden">-10</text>
    <text x="15.15" y="93.06" text-anchor="end" dy="0.35em" id="img-2f634ffe-197" gadfly:scale="10.0" visibility="hidden">-9</text>
    <text x="15.15" y="89.47" text-anchor="end" dy="0.35em" id="img-2f634ffe-198" gadfly:scale="10.0" visibility="hidden">-8</text>
    <text x="15.15" y="85.89" text-anchor="end" dy="0.35em" id="img-2f634ffe-199" gadfly:scale="10.0" visibility="hidden">-7</text>
    <text x="15.15" y="82.3" text-anchor="end" dy="0.35em" id="img-2f634ffe-200" gadfly:scale="10.0" visibility="hidden">-6</text>
    <text x="15.15" y="78.72" text-anchor="end" dy="0.35em" id="img-2f634ffe-201" gadfly:scale="10.0" visibility="hidden">-5</text>
    <text x="15.15" y="75.13" text-anchor="end" dy="0.35em" id="img-2f634ffe-202" gadfly:scale="10.0" visibility="hidden">-4</text>
    <text x="15.15" y="71.54" text-anchor="end" dy="0.35em" id="img-2f634ffe-203" gadfly:scale="10.0" visibility="hidden">-3</text>
    <text x="15.15" y="67.96" text-anchor="end" dy="0.35em" id="img-2f634ffe-204" gadfly:scale="10.0" visibility="hidden">-2</text>
    <text x="15.15" y="64.37" text-anchor="end" dy="0.35em" id="img-2f634ffe-205" gadfly:scale="10.0" visibility="hidden">-1</text>
    <text x="15.15" y="60.79" text-anchor="end" dy="0.35em" id="img-2f634ffe-206" gadfly:scale="10.0" visibility="hidden">0</text>
    <text x="15.15" y="57.2" text-anchor="end" dy="0.35em" id="img-2f634ffe-207" gadfly:scale="10.0" visibility="hidden">1</text>
    <text x="15.15" y="53.61" text-anchor="end" dy="0.35em" id="img-2f634ffe-208" gadfly:scale="10.0" visibility="hidden">2</text>
    <text x="15.15" y="50.03" text-anchor="end" dy="0.35em" id="img-2f634ffe-209" gadfly:scale="10.0" visibility="hidden">3</text>
    <text x="15.15" y="46.44" text-anchor="end" dy="0.35em" id="img-2f634ffe-210" gadfly:scale="10.0" visibility="hidden">4</text>
    <text x="15.15" y="42.86" text-anchor="end" dy="0.35em" id="img-2f634ffe-211" gadfly:scale="10.0" visibility="hidden">5</text>
    <text x="15.15" y="39.27" text-anchor="end" dy="0.35em" id="img-2f634ffe-212" gadfly:scale="10.0" visibility="hidden">6</text>
    <text x="15.15" y="35.69" text-anchor="end" dy="0.35em" id="img-2f634ffe-213" gadfly:scale="10.0" visibility="hidden">7</text>
    <text x="15.15" y="32.1" text-anchor="end" dy="0.35em" id="img-2f634ffe-214" gadfly:scale="10.0" visibility="hidden">8</text>
    <text x="15.15" y="28.51" text-anchor="end" dy="0.35em" id="img-2f634ffe-215" gadfly:scale="10.0" visibility="hidden">9</text>
    <text x="15.15" y="24.93" text-anchor="end" dy="0.35em" id="img-2f634ffe-216" gadfly:scale="10.0" visibility="hidden">10</text>
    <text x="15.15" y="21.34" text-anchor="end" dy="0.35em" id="img-2f634ffe-217" gadfly:scale="10.0" visibility="hidden">11</text>
    <text x="15.15" y="17.76" text-anchor="end" dy="0.35em" id="img-2f634ffe-218" gadfly:scale="10.0" visibility="hidden">12</text>
    <text x="15.15" y="14.17" text-anchor="end" dy="0.35em" id="img-2f634ffe-219" gadfly:scale="10.0" visibility="hidden">13</text>
    <text x="15.15" y="10.59" text-anchor="end" dy="0.35em" id="img-2f634ffe-220" gadfly:scale="10.0" visibility="hidden">14</text>
    <text x="15.15" y="7" text-anchor="end" dy="0.35em" id="img-2f634ffe-221" gadfly:scale="10.0" visibility="hidden">15</text>
    <text x="15.15" y="3.41" text-anchor="end" dy="0.35em" id="img-2f634ffe-222" gadfly:scale="10.0" visibility="hidden">16</text>
    <text x="15.15" y="-0.17" text-anchor="end" dy="0.35em" id="img-2f634ffe-223" gadfly:scale="10.0" visibility="hidden">17</text>
    <text x="15.15" y="-3.76" text-anchor="end" dy="0.35em" id="img-2f634ffe-224" gadfly:scale="10.0" visibility="hidden">18</text>
    <text x="15.15" y="-7.34" text-anchor="end" dy="0.35em" id="img-2f634ffe-225" gadfly:scale="10.0" visibility="hidden">19</text>
    <text x="15.15" y="-10.93" text-anchor="end" dy="0.35em" id="img-2f634ffe-226" gadfly:scale="10.0" visibility="hidden">20</text>
    <text x="15.15" y="-14.51" text-anchor="end" dy="0.35em" id="img-2f634ffe-227" gadfly:scale="10.0" visibility="hidden">21</text>
    <text x="15.15" y="-18.1" text-anchor="end" dy="0.35em" id="img-2f634ffe-228" gadfly:scale="10.0" visibility="hidden">22</text>
    <text x="15.15" y="-21.69" text-anchor="end" dy="0.35em" id="img-2f634ffe-229" gadfly:scale="10.0" visibility="hidden">23</text>
    <text x="15.15" y="-25.27" text-anchor="end" dy="0.35em" id="img-2f634ffe-230" gadfly:scale="10.0" visibility="hidden">24</text>
    <text x="15.15" y="-28.86" text-anchor="end" dy="0.35em" id="img-2f634ffe-231" gadfly:scale="10.0" visibility="hidden">25</text>
    <text x="15.15" y="-32.44" text-anchor="end" dy="0.35em" id="img-2f634ffe-232" gadfly:scale="10.0" visibility="hidden">26</text>
    <text x="15.15" y="-36.03" text-anchor="end" dy="0.35em" id="img-2f634ffe-233" gadfly:scale="10.0" visibility="hidden">27</text>
    <text x="15.15" y="-39.61" text-anchor="end" dy="0.35em" id="img-2f634ffe-234" gadfly:scale="10.0" visibility="hidden">28</text>
    <text x="15.15" y="-43.2" text-anchor="end" dy="0.35em" id="img-2f634ffe-235" gadfly:scale="10.0" visibility="hidden">29</text>
    <text x="15.15" y="-46.79" text-anchor="end" dy="0.35em" id="img-2f634ffe-236" gadfly:scale="10.0" visibility="hidden">30</text>
    <text x="15.15" y="-50.37" text-anchor="end" dy="0.35em" id="img-2f634ffe-237" gadfly:scale="10.0" visibility="hidden">31</text>
    <text x="15.15" y="-53.96" text-anchor="end" dy="0.35em" id="img-2f634ffe-238" gadfly:scale="10.0" visibility="hidden">32</text>
    <text x="15.15" y="-57.54" text-anchor="end" dy="0.35em" id="img-2f634ffe-239" gadfly:scale="10.0" visibility="hidden">33</text>
    <text x="15.15" y="-61.13" text-anchor="end" dy="0.35em" id="img-2f634ffe-240" gadfly:scale="10.0" visibility="hidden">34</text>
    <text x="15.15" y="-64.72" text-anchor="end" dy="0.35em" id="img-2f634ffe-241" gadfly:scale="10.0" visibility="hidden">35</text>
    <text x="15.15" y="204.22" text-anchor="end" dy="0.35em" id="img-2f634ffe-242" gadfly:scale="0.5" visibility="hidden">-40</text>
    <text x="15.15" y="132.5" text-anchor="end" dy="0.35em" id="img-2f634ffe-243" gadfly:scale="0.5" visibility="hidden">-20</text>
    <text x="15.15" y="60.79" text-anchor="end" dy="0.35em" id="img-2f634ffe-244" gadfly:scale="0.5" visibility="hidden">0</text>
    <text x="15.15" y="-10.93" text-anchor="end" dy="0.35em" id="img-2f634ffe-245" gadfly:scale="0.5" visibility="hidden">20</text>
    <text x="15.15" y="-82.64" text-anchor="end" dy="0.35em" id="img-2f634ffe-246" gadfly:scale="0.5" visibility="hidden">40</text>
    <text x="15.15" y="150.43" text-anchor="end" dy="0.35em" id="img-2f634ffe-247" gadfly:scale="5.0" visibility="hidden">-25</text>
    <text x="15.15" y="132.5" text-anchor="end" dy="0.35em" id="img-2f634ffe-248" gadfly:scale="5.0" visibility="hidden">-20</text>
    <text x="15.15" y="114.57" text-anchor="end" dy="0.35em" id="img-2f634ffe-249" gadfly:scale="5.0" visibility="hidden">-15</text>
    <text x="15.15" y="96.64" text-anchor="end" dy="0.35em" id="img-2f634ffe-250" gadfly:scale="5.0" visibility="hidden">-10</text>
    <text x="15.15" y="78.72" text-anchor="end" dy="0.35em" id="img-2f634ffe-251" gadfly:scale="5.0" visibility="hidden">-5</text>
    <text x="15.15" y="60.79" text-anchor="end" dy="0.35em" id="img-2f634ffe-252" gadfly:scale="5.0" visibility="hidden">0</text>
    <text x="15.15" y="42.86" text-anchor="end" dy="0.35em" id="img-2f634ffe-253" gadfly:scale="5.0" visibility="hidden">5</text>
    <text x="15.15" y="24.93" text-anchor="end" dy="0.35em" id="img-2f634ffe-254" gadfly:scale="5.0" visibility="hidden">10</text>
    <text x="15.15" y="7" text-anchor="end" dy="0.35em" id="img-2f634ffe-255" gadfly:scale="5.0" visibility="hidden">15</text>
    <text x="15.15" y="-10.93" text-anchor="end" dy="0.35em" id="img-2f634ffe-256" gadfly:scale="5.0" visibility="hidden">20</text>
    <text x="15.15" y="-28.86" text-anchor="end" dy="0.35em" id="img-2f634ffe-257" gadfly:scale="5.0" visibility="hidden">25</text>
    <text x="15.15" y="-46.79" text-anchor="end" dy="0.35em" id="img-2f634ffe-258" gadfly:scale="5.0" visibility="hidden">30</text>
    <text x="15.15" y="-64.72" text-anchor="end" dy="0.35em" id="img-2f634ffe-259" gadfly:scale="5.0" visibility="hidden">35</text>
  </g>
  <g font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" fill="#564A55" stroke="#000000" stroke-opacity="0.000" id="img-2f634ffe-260">
    <text x="8.81" y="42.86" text-anchor="end" dy="0.35em" id="img-2f634ffe-261">y</text>
  </g>
</g>
<defs>
  <clipPath id="img-2f634ffe-15">
  <path d="M16.15,5 L 117.45 5 117.45 80.72 16.15 80.72" />
</clipPath>
</defs>
<script> <![CDATA[
(function(N){var k=/[\.\/]/,L=/\s*,\s*/,C=function(a,d){return a-d},a,v,y={n:{}},M=function(){for(var a=0,d=this.length;a<d;a++)if("undefined"!=typeof this[a])return this[a]},A=function(){for(var a=this.length;--a;)if("undefined"!=typeof this[a])return this[a]},w=function(k,d){k=String(k);var f=v,n=Array.prototype.slice.call(arguments,2),u=w.listeners(k),p=0,b,q=[],e={},l=[],r=a;l.firstDefined=M;l.lastDefined=A;a=k;for(var s=v=0,x=u.length;s<x;s++)"zIndex"in u[s]&&(q.push(u[s].zIndex),0>u[s].zIndex&&
(e[u[s].zIndex]=u[s]));for(q.sort(C);0>q[p];)if(b=e[q[p++] ],l.push(b.apply(d,n)),v)return v=f,l;for(s=0;s<x;s++)if(b=u[s],"zIndex"in b)if(b.zIndex==q[p]){l.push(b.apply(d,n));if(v)break;do if(p++,(b=e[q[p] ])&&l.push(b.apply(d,n)),v)break;while(b)}else e[b.zIndex]=b;else if(l.push(b.apply(d,n)),v)break;v=f;a=r;return l};w._events=y;w.listeners=function(a){a=a.split(k);var d=y,f,n,u,p,b,q,e,l=[d],r=[];u=0;for(p=a.length;u<p;u++){e=[];b=0;for(q=l.length;b<q;b++)for(d=l[b].n,f=[d[a[u] ],d["*"] ],n=2;n--;)if(d=
f[n])e.push(d),r=r.concat(d.f||[]);l=e}return r};w.on=function(a,d){a=String(a);if("function"!=typeof d)return function(){};for(var f=a.split(L),n=0,u=f.length;n<u;n++)(function(a){a=a.split(k);for(var b=y,f,e=0,l=a.length;e<l;e++)b=b.n,b=b.hasOwnProperty(a[e])&&b[a[e] ]||(b[a[e] ]={n:{}});b.f=b.f||[];e=0;for(l=b.f.length;e<l;e++)if(b.f[e]==d){f=!0;break}!f&&b.f.push(d)})(f[n]);return function(a){+a==+a&&(d.zIndex=+a)}};w.f=function(a){var d=[].slice.call(arguments,1);return function(){w.apply(null,
[a,null].concat(d).concat([].slice.call(arguments,0)))}};w.stop=function(){v=1};w.nt=function(k){return k?(new RegExp("(?:\\.|\\/|^)"+k+"(?:\\.|\\/|$)")).test(a):a};w.nts=function(){return a.split(k)};w.off=w.unbind=function(a,d){if(a){var f=a.split(L);if(1<f.length)for(var n=0,u=f.length;n<u;n++)w.off(f[n],d);else{for(var f=a.split(k),p,b,q,e,l=[y],n=0,u=f.length;n<u;n++)for(e=0;e<l.length;e+=q.length-2){q=[e,1];p=l[e].n;if("*"!=f[n])p[f[n] ]&&q.push(p[f[n] ]);else for(b in p)p.hasOwnProperty(b)&&
q.push(p[b]);l.splice.apply(l,q)}n=0;for(u=l.length;n<u;n++)for(p=l[n];p.n;){if(d){if(p.f){e=0;for(f=p.f.length;e<f;e++)if(p.f[e]==d){p.f.splice(e,1);break}!p.f.length&&delete p.f}for(b in p.n)if(p.n.hasOwnProperty(b)&&p.n[b].f){q=p.n[b].f;e=0;for(f=q.length;e<f;e++)if(q[e]==d){q.splice(e,1);break}!q.length&&delete p.n[b].f}}else for(b in delete p.f,p.n)p.n.hasOwnProperty(b)&&p.n[b].f&&delete p.n[b].f;p=p.n}}}else w._events=y={n:{}}};w.once=function(a,d){var f=function(){w.unbind(a,f);return d.apply(this,
arguments)};return w.on(a,f)};w.version="0.4.2";w.toString=function(){return"You are running Eve 0.4.2"};"undefined"!=typeof module&&module.exports?module.exports=w:"function"===typeof define&&define.amd?define("eve",[],function(){return w}):N.eve=w})(this);
(function(N,k){"function"===typeof define&&define.amd?define("Snap.svg",["eve"],function(L){return k(N,L)}):k(N,N.eve)})(this,function(N,k){var L=function(a){var k={},y=N.requestAnimationFrame||N.webkitRequestAnimationFrame||N.mozRequestAnimationFrame||N.oRequestAnimationFrame||N.msRequestAnimationFrame||function(a){setTimeout(a,16)},M=Array.isArray||function(a){return a instanceof Array||"[object Array]"==Object.prototype.toString.call(a)},A=0,w="M"+(+new Date).toString(36),z=function(a){if(null==
a)return this.s;var b=this.s-a;this.b+=this.dur*b;this.B+=this.dur*b;this.s=a},d=function(a){if(null==a)return this.spd;this.spd=a},f=function(a){if(null==a)return this.dur;this.s=this.s*a/this.dur;this.dur=a},n=function(){delete k[this.id];this.update();a("mina.stop."+this.id,this)},u=function(){this.pdif||(delete k[this.id],this.update(),this.pdif=this.get()-this.b)},p=function(){this.pdif&&(this.b=this.get()-this.pdif,delete this.pdif,k[this.id]=this)},b=function(){var a;if(M(this.start)){a=[];
for(var b=0,e=this.start.length;b<e;b++)a[b]=+this.start[b]+(this.end[b]-this.start[b])*this.easing(this.s)}else a=+this.start+(this.end-this.start)*this.easing(this.s);this.set(a)},q=function(){var l=0,b;for(b in k)if(k.hasOwnProperty(b)){var e=k[b],f=e.get();l++;e.s=(f-e.b)/(e.dur/e.spd);1<=e.s&&(delete k[b],e.s=1,l--,function(b){setTimeout(function(){a("mina.finish."+b.id,b)})}(e));e.update()}l&&y(q)},e=function(a,r,s,x,G,h,J){a={id:w+(A++).toString(36),start:a,end:r,b:s,s:0,dur:x-s,spd:1,get:G,
set:h,easing:J||e.linear,status:z,speed:d,duration:f,stop:n,pause:u,resume:p,update:b};k[a.id]=a;r=0;for(var K in k)if(k.hasOwnProperty(K)&&(r++,2==r))break;1==r&&y(q);return a};e.time=Date.now||function(){return+new Date};e.getById=function(a){return k[a]||null};e.linear=function(a){return a};e.easeout=function(a){return Math.pow(a,1.7)};e.easein=function(a){return Math.pow(a,0.48)};e.easeinout=function(a){if(1==a)return 1;if(0==a)return 0;var b=0.48-a/1.04,e=Math.sqrt(0.1734+b*b);a=e-b;a=Math.pow(Math.abs(a),
1/3)*(0>a?-1:1);b=-e-b;b=Math.pow(Math.abs(b),1/3)*(0>b?-1:1);a=a+b+0.5;return 3*(1-a)*a*a+a*a*a};e.backin=function(a){return 1==a?1:a*a*(2.70158*a-1.70158)};e.backout=function(a){if(0==a)return 0;a-=1;return a*a*(2.70158*a+1.70158)+1};e.elastic=function(a){return a==!!a?a:Math.pow(2,-10*a)*Math.sin(2*(a-0.075)*Math.PI/0.3)+1};e.bounce=function(a){a<1/2.75?a*=7.5625*a:a<2/2.75?(a-=1.5/2.75,a=7.5625*a*a+0.75):a<2.5/2.75?(a-=2.25/2.75,a=7.5625*a*a+0.9375):(a-=2.625/2.75,a=7.5625*a*a+0.984375);return a};
return N.mina=e}("undefined"==typeof k?function(){}:k),C=function(){function a(c,t){if(c){if(c.tagName)return x(c);if(y(c,"array")&&a.set)return a.set.apply(a,c);if(c instanceof e)return c;if(null==t)return c=G.doc.querySelector(c),x(c)}return new s(null==c?"100%":c,null==t?"100%":t)}function v(c,a){if(a){"#text"==c&&(c=G.doc.createTextNode(a.text||""));"string"==typeof c&&(c=v(c));if("string"==typeof a)return"xlink:"==a.substring(0,6)?c.getAttributeNS(m,a.substring(6)):"xml:"==a.substring(0,4)?c.getAttributeNS(la,
a.substring(4)):c.getAttribute(a);for(var da in a)if(a[h](da)){var b=J(a[da]);b?"xlink:"==da.substring(0,6)?c.setAttributeNS(m,da.substring(6),b):"xml:"==da.substring(0,4)?c.setAttributeNS(la,da.substring(4),b):c.setAttribute(da,b):c.removeAttribute(da)}}else c=G.doc.createElementNS(la,c);return c}function y(c,a){a=J.prototype.toLowerCase.call(a);return"finite"==a?isFinite(c):"array"==a&&(c instanceof Array||Array.isArray&&Array.isArray(c))?!0:"null"==a&&null===c||a==typeof c&&null!==c||"object"==
a&&c===Object(c)||$.call(c).slice(8,-1).toLowerCase()==a}function M(c){if("function"==typeof c||Object(c)!==c)return c;var a=new c.constructor,b;for(b in c)c[h](b)&&(a[b]=M(c[b]));return a}function A(c,a,b){function m(){var e=Array.prototype.slice.call(arguments,0),f=e.join("\u2400"),d=m.cache=m.cache||{},l=m.count=m.count||[];if(d[h](f)){a:for(var e=l,l=f,B=0,H=e.length;B<H;B++)if(e[B]===l){e.push(e.splice(B,1)[0]);break a}return b?b(d[f]):d[f]}1E3<=l.length&&delete d[l.shift()];l.push(f);d[f]=c.apply(a,
e);return b?b(d[f]):d[f]}return m}function w(c,a,b,m,e,f){return null==e?(c-=b,a-=m,c||a?(180*I.atan2(-a,-c)/C+540)%360:0):w(c,a,e,f)-w(b,m,e,f)}function z(c){return c%360*C/180}function d(c){var a=[];c=c.replace(/(?:^|\s)(\w+)\(([^)]+)\)/g,function(c,b,m){m=m.split(/\s*,\s*|\s+/);"rotate"==b&&1==m.length&&m.push(0,0);"scale"==b&&(2<m.length?m=m.slice(0,2):2==m.length&&m.push(0,0),1==m.length&&m.push(m[0],0,0));"skewX"==b?a.push(["m",1,0,I.tan(z(m[0])),1,0,0]):"skewY"==b?a.push(["m",1,I.tan(z(m[0])),
0,1,0,0]):a.push([b.charAt(0)].concat(m));return c});return a}function f(c,t){var b=O(c),m=new a.Matrix;if(b)for(var e=0,f=b.length;e<f;e++){var h=b[e],d=h.length,B=J(h[0]).toLowerCase(),H=h[0]!=B,l=H?m.invert():0,E;"t"==B&&2==d?m.translate(h[1],0):"t"==B&&3==d?H?(d=l.x(0,0),B=l.y(0,0),H=l.x(h[1],h[2]),l=l.y(h[1],h[2]),m.translate(H-d,l-B)):m.translate(h[1],h[2]):"r"==B?2==d?(E=E||t,m.rotate(h[1],E.x+E.width/2,E.y+E.height/2)):4==d&&(H?(H=l.x(h[2],h[3]),l=l.y(h[2],h[3]),m.rotate(h[1],H,l)):m.rotate(h[1],
h[2],h[3])):"s"==B?2==d||3==d?(E=E||t,m.scale(h[1],h[d-1],E.x+E.width/2,E.y+E.height/2)):4==d?H?(H=l.x(h[2],h[3]),l=l.y(h[2],h[3]),m.scale(h[1],h[1],H,l)):m.scale(h[1],h[1],h[2],h[3]):5==d&&(H?(H=l.x(h[3],h[4]),l=l.y(h[3],h[4]),m.scale(h[1],h[2],H,l)):m.scale(h[1],h[2],h[3],h[4])):"m"==B&&7==d&&m.add(h[1],h[2],h[3],h[4],h[5],h[6])}return m}function n(c,t){if(null==t){var m=!0;t="linearGradient"==c.type||"radialGradient"==c.type?c.node.getAttribute("gradientTransform"):"pattern"==c.type?c.node.getAttribute("patternTransform"):
c.node.getAttribute("transform");if(!t)return new a.Matrix;t=d(t)}else t=a._.rgTransform.test(t)?J(t).replace(/\.{3}|\u2026/g,c._.transform||aa):d(t),y(t,"array")&&(t=a.path?a.path.toString.call(t):J(t)),c._.transform=t;var b=f(t,c.getBBox(1));if(m)return b;c.matrix=b}function u(c){c=c.node.ownerSVGElement&&x(c.node.ownerSVGElement)||c.node.parentNode&&x(c.node.parentNode)||a.select("svg")||a(0,0);var t=c.select("defs"),t=null==t?!1:t.node;t||(t=r("defs",c.node).node);return t}function p(c){return c.node.ownerSVGElement&&
x(c.node.ownerSVGElement)||a.select("svg")}function b(c,a,m){function b(c){if(null==c)return aa;if(c==+c)return c;v(B,{width:c});try{return B.getBBox().width}catch(a){return 0}}function h(c){if(null==c)return aa;if(c==+c)return c;v(B,{height:c});try{return B.getBBox().height}catch(a){return 0}}function e(b,B){null==a?d[b]=B(c.attr(b)||0):b==a&&(d=B(null==m?c.attr(b)||0:m))}var f=p(c).node,d={},B=f.querySelector(".svg---mgr");B||(B=v("rect"),v(B,{x:-9E9,y:-9E9,width:10,height:10,"class":"svg---mgr",
fill:"none"}),f.appendChild(B));switch(c.type){case "rect":e("rx",b),e("ry",h);case "image":e("width",b),e("height",h);case "text":e("x",b);e("y",h);break;case "circle":e("cx",b);e("cy",h);e("r",b);break;case "ellipse":e("cx",b);e("cy",h);e("rx",b);e("ry",h);break;case "line":e("x1",b);e("x2",b);e("y1",h);e("y2",h);break;case "marker":e("refX",b);e("markerWidth",b);e("refY",h);e("markerHeight",h);break;case "radialGradient":e("fx",b);e("fy",h);break;case "tspan":e("dx",b);e("dy",h);break;default:e(a,
b)}f.removeChild(B);return d}function q(c){y(c,"array")||(c=Array.prototype.slice.call(arguments,0));for(var a=0,b=0,m=this.node;this[a];)delete this[a++];for(a=0;a<c.length;a++)"set"==c[a].type?c[a].forEach(function(c){m.appendChild(c.node)}):m.appendChild(c[a].node);for(var h=m.childNodes,a=0;a<h.length;a++)this[b++]=x(h[a]);return this}function e(c){if(c.snap in E)return E[c.snap];var a=this.id=V(),b;try{b=c.ownerSVGElement}catch(m){}this.node=c;b&&(this.paper=new s(b));this.type=c.tagName;this.anims=
{};this._={transform:[]};c.snap=a;E[a]=this;"g"==this.type&&(this.add=q);if(this.type in{g:1,mask:1,pattern:1})for(var e in s.prototype)s.prototype[h](e)&&(this[e]=s.prototype[e])}function l(c){this.node=c}function r(c,a){var b=v(c);a.appendChild(b);return x(b)}function s(c,a){var b,m,f,d=s.prototype;if(c&&"svg"==c.tagName){if(c.snap in E)return E[c.snap];var l=c.ownerDocument;b=new e(c);m=c.getElementsByTagName("desc")[0];f=c.getElementsByTagName("defs")[0];m||(m=v("desc"),m.appendChild(l.createTextNode("Created with Snap")),
b.node.appendChild(m));f||(f=v("defs"),b.node.appendChild(f));b.defs=f;for(var ca in d)d[h](ca)&&(b[ca]=d[ca]);b.paper=b.root=b}else b=r("svg",G.doc.body),v(b.node,{height:a,version:1.1,width:c,xmlns:la});return b}function x(c){return!c||c instanceof e||c instanceof l?c:c.tagName&&"svg"==c.tagName.toLowerCase()?new s(c):c.tagName&&"object"==c.tagName.toLowerCase()&&"image/svg+xml"==c.type?new s(c.contentDocument.getElementsByTagName("svg")[0]):new e(c)}a.version="0.3.0";a.toString=function(){return"Snap v"+
this.version};a._={};var G={win:N,doc:N.document};a._.glob=G;var h="hasOwnProperty",J=String,K=parseFloat,U=parseInt,I=Math,P=I.max,Q=I.min,Y=I.abs,C=I.PI,aa="",$=Object.prototype.toString,F=/^\s*((#[a-f\d]{6})|(#[a-f\d]{3})|rgba?\(\s*([\d\.]+%?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+%?(?:\s*,\s*[\d\.]+%?)?)\s*\)|hsba?\(\s*([\d\.]+(?:deg|\xb0|%)?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+(?:%?\s*,\s*[\d\.]+)?%?)\s*\)|hsla?\(\s*([\d\.]+(?:deg|\xb0|%)?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+(?:%?\s*,\s*[\d\.]+)?%?)\s*\))\s*$/i;a._.separator=
RegExp("[,\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]+");var S=RegExp("[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*"),X={hs:1,rg:1},W=RegExp("([a-z])[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029,]*((-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*)+)",
"ig"),ma=RegExp("([rstm])[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029,]*((-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*)+)","ig"),Z=RegExp("(-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?)[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*",
"ig"),na=0,ba="S"+(+new Date).toString(36),V=function(){return ba+(na++).toString(36)},m="http://www.w3.org/1999/xlink",la="http://www.w3.org/2000/svg",E={},ca=a.url=function(c){return"url('#"+c+"')"};a._.$=v;a._.id=V;a.format=function(){var c=/\{([^\}]+)\}/g,a=/(?:(?:^|\.)(.+?)(?=\[|\.|$|\()|\[('|")(.+?)\2\])(\(\))?/g,b=function(c,b,m){var h=m;b.replace(a,function(c,a,b,m,t){a=a||m;h&&(a in h&&(h=h[a]),"function"==typeof h&&t&&(h=h()))});return h=(null==h||h==m?c:h)+""};return function(a,m){return J(a).replace(c,
function(c,a){return b(c,a,m)})}}();a._.clone=M;a._.cacher=A;a.rad=z;a.deg=function(c){return 180*c/C%360};a.angle=w;a.is=y;a.snapTo=function(c,a,b){b=y(b,"finite")?b:10;if(y(c,"array"))for(var m=c.length;m--;){if(Y(c[m]-a)<=b)return c[m]}else{c=+c;m=a%c;if(m<b)return a-m;if(m>c-b)return a-m+c}return a};a.getRGB=A(function(c){if(!c||(c=J(c)).indexOf("-")+1)return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka};if("none"==c)return{r:-1,g:-1,b:-1,hex:"none",toString:ka};!X[h](c.toLowerCase().substring(0,
2))&&"#"!=c.charAt()&&(c=T(c));if(!c)return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka};var b,m,e,f,d;if(c=c.match(F)){c[2]&&(e=U(c[2].substring(5),16),m=U(c[2].substring(3,5),16),b=U(c[2].substring(1,3),16));c[3]&&(e=U((d=c[3].charAt(3))+d,16),m=U((d=c[3].charAt(2))+d,16),b=U((d=c[3].charAt(1))+d,16));c[4]&&(d=c[4].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b*=2.55),m=K(d[1]),"%"==d[1].slice(-1)&&(m*=2.55),e=K(d[2]),"%"==d[2].slice(-1)&&(e*=2.55),"rgba"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),
d[3]&&"%"==d[3].slice(-1)&&(f/=100));if(c[5])return d=c[5].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b/=100),m=K(d[1]),"%"==d[1].slice(-1)&&(m/=100),e=K(d[2]),"%"==d[2].slice(-1)&&(e/=100),"deg"!=d[0].slice(-3)&&"\u00b0"!=d[0].slice(-1)||(b/=360),"hsba"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),d[3]&&"%"==d[3].slice(-1)&&(f/=100),a.hsb2rgb(b,m,e,f);if(c[6])return d=c[6].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b/=100),m=K(d[1]),"%"==d[1].slice(-1)&&(m/=100),e=K(d[2]),"%"==d[2].slice(-1)&&(e/=100),
"deg"!=d[0].slice(-3)&&"\u00b0"!=d[0].slice(-1)||(b/=360),"hsla"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),d[3]&&"%"==d[3].slice(-1)&&(f/=100),a.hsl2rgb(b,m,e,f);b=Q(I.round(b),255);m=Q(I.round(m),255);e=Q(I.round(e),255);f=Q(P(f,0),1);c={r:b,g:m,b:e,toString:ka};c.hex="#"+(16777216|e|m<<8|b<<16).toString(16).slice(1);c.opacity=y(f,"finite")?f:1;return c}return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka}},a);a.hsb=A(function(c,b,m){return a.hsb2rgb(c,b,m).hex});a.hsl=A(function(c,b,m){return a.hsl2rgb(c,
b,m).hex});a.rgb=A(function(c,a,b,m){if(y(m,"finite")){var e=I.round;return"rgba("+[e(c),e(a),e(b),+m.toFixed(2)]+")"}return"#"+(16777216|b|a<<8|c<<16).toString(16).slice(1)});var T=function(c){var a=G.doc.getElementsByTagName("head")[0]||G.doc.getElementsByTagName("svg")[0];T=A(function(c){if("red"==c.toLowerCase())return"rgb(255, 0, 0)";a.style.color="rgb(255, 0, 0)";a.style.color=c;c=G.doc.defaultView.getComputedStyle(a,aa).getPropertyValue("color");return"rgb(255, 0, 0)"==c?null:c});return T(c)},
qa=function(){return"hsb("+[this.h,this.s,this.b]+")"},ra=function(){return"hsl("+[this.h,this.s,this.l]+")"},ka=function(){return 1==this.opacity||null==this.opacity?this.hex:"rgba("+[this.r,this.g,this.b,this.opacity]+")"},D=function(c,b,m){null==b&&y(c,"object")&&"r"in c&&"g"in c&&"b"in c&&(m=c.b,b=c.g,c=c.r);null==b&&y(c,string)&&(m=a.getRGB(c),c=m.r,b=m.g,m=m.b);if(1<c||1<b||1<m)c/=255,b/=255,m/=255;return[c,b,m]},oa=function(c,b,m,e){c=I.round(255*c);b=I.round(255*b);m=I.round(255*m);c={r:c,
g:b,b:m,opacity:y(e,"finite")?e:1,hex:a.rgb(c,b,m),toString:ka};y(e,"finite")&&(c.opacity=e);return c};a.color=function(c){var b;y(c,"object")&&"h"in c&&"s"in c&&"b"in c?(b=a.hsb2rgb(c),c.r=b.r,c.g=b.g,c.b=b.b,c.opacity=1,c.hex=b.hex):y(c,"object")&&"h"in c&&"s"in c&&"l"in c?(b=a.hsl2rgb(c),c.r=b.r,c.g=b.g,c.b=b.b,c.opacity=1,c.hex=b.hex):(y(c,"string")&&(c=a.getRGB(c)),y(c,"object")&&"r"in c&&"g"in c&&"b"in c&&!("error"in c)?(b=a.rgb2hsl(c),c.h=b.h,c.s=b.s,c.l=b.l,b=a.rgb2hsb(c),c.v=b.b):(c={hex:"none"},
c.r=c.g=c.b=c.h=c.s=c.v=c.l=-1,c.error=1));c.toString=ka;return c};a.hsb2rgb=function(c,a,b,m){y(c,"object")&&"h"in c&&"s"in c&&"b"in c&&(b=c.b,a=c.s,c=c.h,m=c.o);var e,h,d;c=360*c%360/60;d=b*a;a=d*(1-Y(c%2-1));b=e=h=b-d;c=~~c;b+=[d,a,0,0,a,d][c];e+=[a,d,d,a,0,0][c];h+=[0,0,a,d,d,a][c];return oa(b,e,h,m)};a.hsl2rgb=function(c,a,b,m){y(c,"object")&&"h"in c&&"s"in c&&"l"in c&&(b=c.l,a=c.s,c=c.h);if(1<c||1<a||1<b)c/=360,a/=100,b/=100;var e,h,d;c=360*c%360/60;d=2*a*(0.5>b?b:1-b);a=d*(1-Y(c%2-1));b=e=
h=b-d/2;c=~~c;b+=[d,a,0,0,a,d][c];e+=[a,d,d,a,0,0][c];h+=[0,0,a,d,d,a][c];return oa(b,e,h,m)};a.rgb2hsb=function(c,a,b){b=D(c,a,b);c=b[0];a=b[1];b=b[2];var m,e;m=P(c,a,b);e=m-Q(c,a,b);c=((0==e?0:m==c?(a-b)/e:m==a?(b-c)/e+2:(c-a)/e+4)+360)%6*60/360;return{h:c,s:0==e?0:e/m,b:m,toString:qa}};a.rgb2hsl=function(c,a,b){b=D(c,a,b);c=b[0];a=b[1];b=b[2];var m,e,h;m=P(c,a,b);e=Q(c,a,b);h=m-e;c=((0==h?0:m==c?(a-b)/h:m==a?(b-c)/h+2:(c-a)/h+4)+360)%6*60/360;m=(m+e)/2;return{h:c,s:0==h?0:0.5>m?h/(2*m):h/(2-2*
m),l:m,toString:ra}};a.parsePathString=function(c){if(!c)return null;var b=a.path(c);if(b.arr)return a.path.clone(b.arr);var m={a:7,c:6,o:2,h:1,l:2,m:2,r:4,q:4,s:4,t:2,v:1,u:3,z:0},e=[];y(c,"array")&&y(c[0],"array")&&(e=a.path.clone(c));e.length||J(c).replace(W,function(c,a,b){var h=[];c=a.toLowerCase();b.replace(Z,function(c,a){a&&h.push(+a)});"m"==c&&2<h.length&&(e.push([a].concat(h.splice(0,2))),c="l",a="m"==a?"l":"L");"o"==c&&1==h.length&&e.push([a,h[0] ]);if("r"==c)e.push([a].concat(h));else for(;h.length>=
m[c]&&(e.push([a].concat(h.splice(0,m[c]))),m[c]););});e.toString=a.path.toString;b.arr=a.path.clone(e);return e};var O=a.parseTransformString=function(c){if(!c)return null;var b=[];y(c,"array")&&y(c[0],"array")&&(b=a.path.clone(c));b.length||J(c).replace(ma,function(c,a,m){var e=[];a.toLowerCase();m.replace(Z,function(c,a){a&&e.push(+a)});b.push([a].concat(e))});b.toString=a.path.toString;return b};a._.svgTransform2string=d;a._.rgTransform=RegExp("^[a-z][\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*-?\\.?\\d",
"i");a._.transform2matrix=f;a._unit2px=b;a._.getSomeDefs=u;a._.getSomeSVG=p;a.select=function(c){return x(G.doc.querySelector(c))};a.selectAll=function(c){c=G.doc.querySelectorAll(c);for(var b=(a.set||Array)(),m=0;m<c.length;m++)b.push(x(c[m]));return b};setInterval(function(){for(var c in E)if(E[h](c)){var a=E[c],b=a.node;("svg"!=a.type&&!b.ownerSVGElement||"svg"==a.type&&(!b.parentNode||"ownerSVGElement"in b.parentNode&&!b.ownerSVGElement))&&delete E[c]}},1E4);(function(c){function m(c){function a(c,
b){var m=v(c.node,b);(m=(m=m&&m.match(d))&&m[2])&&"#"==m.charAt()&&(m=m.substring(1))&&(f[m]=(f[m]||[]).concat(function(a){var m={};m[b]=ca(a);v(c.node,m)}))}function b(c){var a=v(c.node,"xlink:href");a&&"#"==a.charAt()&&(a=a.substring(1))&&(f[a]=(f[a]||[]).concat(function(a){c.attr("xlink:href","#"+a)}))}var e=c.selectAll("*"),h,d=/^\s*url\(("|'|)(.*)\1\)\s*$/;c=[];for(var f={},l=0,E=e.length;l<E;l++){h=e[l];a(h,"fill");a(h,"stroke");a(h,"filter");a(h,"mask");a(h,"clip-path");b(h);var t=v(h.node,
"id");t&&(v(h.node,{id:h.id}),c.push({old:t,id:h.id}))}l=0;for(E=c.length;l<E;l++)if(e=f[c[l].old])for(h=0,t=e.length;h<t;h++)e[h](c[l].id)}function e(c,a,b){return function(m){m=m.slice(c,a);1==m.length&&(m=m[0]);return b?b(m):m}}function d(c){return function(){var a=c?"<"+this.type:"",b=this.node.attributes,m=this.node.childNodes;if(c)for(var e=0,h=b.length;e<h;e++)a+=" "+b[e].name+'="'+b[e].value.replace(/"/g,'\\"')+'"';if(m.length){c&&(a+=">");e=0;for(h=m.length;e<h;e++)3==m[e].nodeType?a+=m[e].nodeValue:
1==m[e].nodeType&&(a+=x(m[e]).toString());c&&(a+="</"+this.type+">")}else c&&(a+="/>");return a}}c.attr=function(c,a){if(!c)return this;if(y(c,"string"))if(1<arguments.length){var b={};b[c]=a;c=b}else return k("snap.util.getattr."+c,this).firstDefined();for(var m in c)c[h](m)&&k("snap.util.attr."+m,this,c[m]);return this};c.getBBox=function(c){if(!a.Matrix||!a.path)return this.node.getBBox();var b=this,m=new a.Matrix;if(b.removed)return a._.box();for(;"use"==b.type;)if(c||(m=m.add(b.transform().localMatrix.translate(b.attr("x")||
0,b.attr("y")||0))),b.original)b=b.original;else var e=b.attr("xlink:href"),b=b.original=b.node.ownerDocument.getElementById(e.substring(e.indexOf("#")+1));var e=b._,h=a.path.get[b.type]||a.path.get.deflt;try{if(c)return e.bboxwt=h?a.path.getBBox(b.realPath=h(b)):a._.box(b.node.getBBox()),a._.box(e.bboxwt);b.realPath=h(b);b.matrix=b.transform().localMatrix;e.bbox=a.path.getBBox(a.path.map(b.realPath,m.add(b.matrix)));return a._.box(e.bbox)}catch(d){return a._.box()}};var f=function(){return this.string};
c.transform=function(c){var b=this._;if(null==c){var m=this;c=new a.Matrix(this.node.getCTM());for(var e=n(this),h=[e],d=new a.Matrix,l=e.toTransformString(),b=J(e)==J(this.matrix)?J(b.transform):l;"svg"!=m.type&&(m=m.parent());)h.push(n(m));for(m=h.length;m--;)d.add(h[m]);return{string:b,globalMatrix:c,totalMatrix:d,localMatrix:e,diffMatrix:c.clone().add(e.invert()),global:c.toTransformString(),total:d.toTransformString(),local:l,toString:f}}c instanceof a.Matrix?this.matrix=c:n(this,c);this.node&&
("linearGradient"==this.type||"radialGradient"==this.type?v(this.node,{gradientTransform:this.matrix}):"pattern"==this.type?v(this.node,{patternTransform:this.matrix}):v(this.node,{transform:this.matrix}));return this};c.parent=function(){return x(this.node.parentNode)};c.append=c.add=function(c){if(c){if("set"==c.type){var a=this;c.forEach(function(c){a.add(c)});return this}c=x(c);this.node.appendChild(c.node);c.paper=this.paper}return this};c.appendTo=function(c){c&&(c=x(c),c.append(this));return this};
c.prepend=function(c){if(c){if("set"==c.type){var a=this,b;c.forEach(function(c){b?b.after(c):a.prepend(c);b=c});return this}c=x(c);var m=c.parent();this.node.insertBefore(c.node,this.node.firstChild);this.add&&this.add();c.paper=this.paper;this.parent()&&this.parent().add();m&&m.add()}return this};c.prependTo=function(c){c=x(c);c.prepend(this);return this};c.before=function(c){if("set"==c.type){var a=this;c.forEach(function(c){var b=c.parent();a.node.parentNode.insertBefore(c.node,a.node);b&&b.add()});
this.parent().add();return this}c=x(c);var b=c.parent();this.node.parentNode.insertBefore(c.node,this.node);this.parent()&&this.parent().add();b&&b.add();c.paper=this.paper;return this};c.after=function(c){c=x(c);var a=c.parent();this.node.nextSibling?this.node.parentNode.insertBefore(c.node,this.node.nextSibling):this.node.parentNode.appendChild(c.node);this.parent()&&this.parent().add();a&&a.add();c.paper=this.paper;return this};c.insertBefore=function(c){c=x(c);var a=this.parent();c.node.parentNode.insertBefore(this.node,
c.node);this.paper=c.paper;a&&a.add();c.parent()&&c.parent().add();return this};c.insertAfter=function(c){c=x(c);var a=this.parent();c.node.parentNode.insertBefore(this.node,c.node.nextSibling);this.paper=c.paper;a&&a.add();c.parent()&&c.parent().add();return this};c.remove=function(){var c=this.parent();this.node.parentNode&&this.node.parentNode.removeChild(this.node);delete this.paper;this.removed=!0;c&&c.add();return this};c.select=function(c){return x(this.node.querySelector(c))};c.selectAll=
function(c){c=this.node.querySelectorAll(c);for(var b=(a.set||Array)(),m=0;m<c.length;m++)b.push(x(c[m]));return b};c.asPX=function(c,a){null==a&&(a=this.attr(c));return+b(this,c,a)};c.use=function(){var c,a=this.node.id;a||(a=this.id,v(this.node,{id:a}));c="linearGradient"==this.type||"radialGradient"==this.type||"pattern"==this.type?r(this.type,this.node.parentNode):r("use",this.node.parentNode);v(c.node,{"xlink:href":"#"+a});c.original=this;return c};var l=/\S+/g;c.addClass=function(c){var a=(c||
"").match(l)||[];c=this.node;var b=c.className.baseVal,m=b.match(l)||[],e,h,d;if(a.length){for(e=0;d=a[e++];)h=m.indexOf(d),~h||m.push(d);a=m.join(" ");b!=a&&(c.className.baseVal=a)}return this};c.removeClass=function(c){var a=(c||"").match(l)||[];c=this.node;var b=c.className.baseVal,m=b.match(l)||[],e,h;if(m.length){for(e=0;h=a[e++];)h=m.indexOf(h),~h&&m.splice(h,1);a=m.join(" ");b!=a&&(c.className.baseVal=a)}return this};c.hasClass=function(c){return!!~(this.node.className.baseVal.match(l)||[]).indexOf(c)};
c.toggleClass=function(c,a){if(null!=a)return a?this.addClass(c):this.removeClass(c);var b=(c||"").match(l)||[],m=this.node,e=m.className.baseVal,h=e.match(l)||[],d,f,E;for(d=0;E=b[d++];)f=h.indexOf(E),~f?h.splice(f,1):h.push(E);b=h.join(" ");e!=b&&(m.className.baseVal=b);return this};c.clone=function(){var c=x(this.node.cloneNode(!0));v(c.node,"id")&&v(c.node,{id:c.id});m(c);c.insertAfter(this);return c};c.toDefs=function(){u(this).appendChild(this.node);return this};c.pattern=c.toPattern=function(c,
a,b,m){var e=r("pattern",u(this));null==c&&(c=this.getBBox());y(c,"object")&&"x"in c&&(a=c.y,b=c.width,m=c.height,c=c.x);v(e.node,{x:c,y:a,width:b,height:m,patternUnits:"userSpaceOnUse",id:e.id,viewBox:[c,a,b,m].join(" ")});e.node.appendChild(this.node);return e};c.marker=function(c,a,b,m,e,h){var d=r("marker",u(this));null==c&&(c=this.getBBox());y(c,"object")&&"x"in c&&(a=c.y,b=c.width,m=c.height,e=c.refX||c.cx,h=c.refY||c.cy,c=c.x);v(d.node,{viewBox:[c,a,b,m].join(" "),markerWidth:b,markerHeight:m,
orient:"auto",refX:e||0,refY:h||0,id:d.id});d.node.appendChild(this.node);return d};var E=function(c,a,b,m){"function"!=typeof b||b.length||(m=b,b=L.linear);this.attr=c;this.dur=a;b&&(this.easing=b);m&&(this.callback=m)};a._.Animation=E;a.animation=function(c,a,b,m){return new E(c,a,b,m)};c.inAnim=function(){var c=[],a;for(a in this.anims)this.anims[h](a)&&function(a){c.push({anim:new E(a._attrs,a.dur,a.easing,a._callback),mina:a,curStatus:a.status(),status:function(c){return a.status(c)},stop:function(){a.stop()}})}(this.anims[a]);
return c};a.animate=function(c,a,b,m,e,h){"function"!=typeof e||e.length||(h=e,e=L.linear);var d=L.time();c=L(c,a,d,d+m,L.time,b,e);h&&k.once("mina.finish."+c.id,h);return c};c.stop=function(){for(var c=this.inAnim(),a=0,b=c.length;a<b;a++)c[a].stop();return this};c.animate=function(c,a,b,m){"function"!=typeof b||b.length||(m=b,b=L.linear);c instanceof E&&(m=c.callback,b=c.easing,a=b.dur,c=c.attr);var d=[],f=[],l={},t,ca,n,T=this,q;for(q in c)if(c[h](q)){T.equal?(n=T.equal(q,J(c[q])),t=n.from,ca=
n.to,n=n.f):(t=+T.attr(q),ca=+c[q]);var la=y(t,"array")?t.length:1;l[q]=e(d.length,d.length+la,n);d=d.concat(t);f=f.concat(ca)}t=L.time();var p=L(d,f,t,t+a,L.time,function(c){var a={},b;for(b in l)l[h](b)&&(a[b]=l[b](c));T.attr(a)},b);T.anims[p.id]=p;p._attrs=c;p._callback=m;k("snap.animcreated."+T.id,p);k.once("mina.finish."+p.id,function(){delete T.anims[p.id];m&&m.call(T)});k.once("mina.stop."+p.id,function(){delete T.anims[p.id]});return T};var T={};c.data=function(c,b){var m=T[this.id]=T[this.id]||
{};if(0==arguments.length)return k("snap.data.get."+this.id,this,m,null),m;if(1==arguments.length){if(a.is(c,"object")){for(var e in c)c[h](e)&&this.data(e,c[e]);return this}k("snap.data.get."+this.id,this,m[c],c);return m[c]}m[c]=b;k("snap.data.set."+this.id,this,b,c);return this};c.removeData=function(c){null==c?T[this.id]={}:T[this.id]&&delete T[this.id][c];return this};c.outerSVG=c.toString=d(1);c.innerSVG=d()})(e.prototype);a.parse=function(c){var a=G.doc.createDocumentFragment(),b=!0,m=G.doc.createElement("div");
c=J(c);c.match(/^\s*<\s*svg(?:\s|>)/)||(c="<svg>"+c+"</svg>",b=!1);m.innerHTML=c;if(c=m.getElementsByTagName("svg")[0])if(b)a=c;else for(;c.firstChild;)a.appendChild(c.firstChild);m.innerHTML=aa;return new l(a)};l.prototype.select=e.prototype.select;l.prototype.selectAll=e.prototype.selectAll;a.fragment=function(){for(var c=Array.prototype.slice.call(arguments,0),b=G.doc.createDocumentFragment(),m=0,e=c.length;m<e;m++){var h=c[m];h.node&&h.node.nodeType&&b.appendChild(h.node);h.nodeType&&b.appendChild(h);
"string"==typeof h&&b.appendChild(a.parse(h).node)}return new l(b)};a._.make=r;a._.wrap=x;s.prototype.el=function(c,a){var b=r(c,this.node);a&&b.attr(a);return b};k.on("snap.util.getattr",function(){var c=k.nt(),c=c.substring(c.lastIndexOf(".")+1),a=c.replace(/[A-Z]/g,function(c){return"-"+c.toLowerCase()});return pa[h](a)?this.node.ownerDocument.defaultView.getComputedStyle(this.node,null).getPropertyValue(a):v(this.node,c)});var pa={"alignment-baseline":0,"baseline-shift":0,clip:0,"clip-path":0,
"clip-rule":0,color:0,"color-interpolation":0,"color-interpolation-filters":0,"color-profile":0,"color-rendering":0,cursor:0,direction:0,display:0,"dominant-baseline":0,"enable-background":0,fill:0,"fill-opacity":0,"fill-rule":0,filter:0,"flood-color":0,"flood-opacity":0,font:0,"font-family":0,"font-size":0,"font-size-adjust":0,"font-stretch":0,"font-style":0,"font-variant":0,"font-weight":0,"glyph-orientation-horizontal":0,"glyph-orientation-vertical":0,"image-rendering":0,kerning:0,"letter-spacing":0,
"lighting-color":0,marker:0,"marker-end":0,"marker-mid":0,"marker-start":0,mask:0,opacity:0,overflow:0,"pointer-events":0,"shape-rendering":0,"stop-color":0,"stop-opacity":0,stroke:0,"stroke-dasharray":0,"stroke-dashoffset":0,"stroke-linecap":0,"stroke-linejoin":0,"stroke-miterlimit":0,"stroke-opacity":0,"stroke-width":0,"text-anchor":0,"text-decoration":0,"text-rendering":0,"unicode-bidi":0,visibility:0,"word-spacing":0,"writing-mode":0};k.on("snap.util.attr",function(c){var a=k.nt(),b={},a=a.substring(a.lastIndexOf(".")+
1);b[a]=c;var m=a.replace(/-(\w)/gi,function(c,a){return a.toUpperCase()}),a=a.replace(/[A-Z]/g,function(c){return"-"+c.toLowerCase()});pa[h](a)?this.node.style[m]=null==c?aa:c:v(this.node,b)});a.ajax=function(c,a,b,m){var e=new XMLHttpRequest,h=V();if(e){if(y(a,"function"))m=b,b=a,a=null;else if(y(a,"object")){var d=[],f;for(f in a)a.hasOwnProperty(f)&&d.push(encodeURIComponent(f)+"="+encodeURIComponent(a[f]));a=d.join("&")}e.open(a?"POST":"GET",c,!0);a&&(e.setRequestHeader("X-Requested-With","XMLHttpRequest"),
e.setRequestHeader("Content-type","application/x-www-form-urlencoded"));b&&(k.once("snap.ajax."+h+".0",b),k.once("snap.ajax."+h+".200",b),k.once("snap.ajax."+h+".304",b));e.onreadystatechange=function(){4==e.readyState&&k("snap.ajax."+h+"."+e.status,m,e)};if(4==e.readyState)return e;e.send(a);return e}};a.load=function(c,b,m){a.ajax(c,function(c){c=a.parse(c.responseText);m?b.call(m,c):b(c)})};a.getElementByPoint=function(c,a){var b,m,e=G.doc.elementFromPoint(c,a);if(G.win.opera&&"svg"==e.tagName){b=
e;m=b.getBoundingClientRect();b=b.ownerDocument;var h=b.body,d=b.documentElement;b=m.top+(g.win.pageYOffset||d.scrollTop||h.scrollTop)-(d.clientTop||h.clientTop||0);m=m.left+(g.win.pageXOffset||d.scrollLeft||h.scrollLeft)-(d.clientLeft||h.clientLeft||0);h=e.createSVGRect();h.x=c-m;h.y=a-b;h.width=h.height=1;b=e.getIntersectionList(h,null);b.length&&(e=b[b.length-1])}return e?x(e):null};a.plugin=function(c){c(a,e,s,G,l)};return G.win.Snap=a}();C.plugin(function(a,k,y,M,A){function w(a,d,f,b,q,e){null==
d&&"[object SVGMatrix]"==z.call(a)?(this.a=a.a,this.b=a.b,this.c=a.c,this.d=a.d,this.e=a.e,this.f=a.f):null!=a?(this.a=+a,this.b=+d,this.c=+f,this.d=+b,this.e=+q,this.f=+e):(this.a=1,this.c=this.b=0,this.d=1,this.f=this.e=0)}var z=Object.prototype.toString,d=String,f=Math;(function(n){function k(a){return a[0]*a[0]+a[1]*a[1]}function p(a){var d=f.sqrt(k(a));a[0]&&(a[0]/=d);a[1]&&(a[1]/=d)}n.add=function(a,d,e,f,n,p){var k=[[],[],[] ],u=[[this.a,this.c,this.e],[this.b,this.d,this.f],[0,0,1] ];d=[[a,
e,n],[d,f,p],[0,0,1] ];a&&a instanceof w&&(d=[[a.a,a.c,a.e],[a.b,a.d,a.f],[0,0,1] ]);for(a=0;3>a;a++)for(e=0;3>e;e++){for(f=n=0;3>f;f++)n+=u[a][f]*d[f][e];k[a][e]=n}this.a=k[0][0];this.b=k[1][0];this.c=k[0][1];this.d=k[1][1];this.e=k[0][2];this.f=k[1][2];return this};n.invert=function(){var a=this.a*this.d-this.b*this.c;return new w(this.d/a,-this.b/a,-this.c/a,this.a/a,(this.c*this.f-this.d*this.e)/a,(this.b*this.e-this.a*this.f)/a)};n.clone=function(){return new w(this.a,this.b,this.c,this.d,this.e,
this.f)};n.translate=function(a,d){return this.add(1,0,0,1,a,d)};n.scale=function(a,d,e,f){null==d&&(d=a);(e||f)&&this.add(1,0,0,1,e,f);this.add(a,0,0,d,0,0);(e||f)&&this.add(1,0,0,1,-e,-f);return this};n.rotate=function(b,d,e){b=a.rad(b);d=d||0;e=e||0;var l=+f.cos(b).toFixed(9);b=+f.sin(b).toFixed(9);this.add(l,b,-b,l,d,e);return this.add(1,0,0,1,-d,-e)};n.x=function(a,d){return a*this.a+d*this.c+this.e};n.y=function(a,d){return a*this.b+d*this.d+this.f};n.get=function(a){return+this[d.fromCharCode(97+
a)].toFixed(4)};n.toString=function(){return"matrix("+[this.get(0),this.get(1),this.get(2),this.get(3),this.get(4),this.get(5)].join()+")"};n.offset=function(){return[this.e.toFixed(4),this.f.toFixed(4)]};n.determinant=function(){return this.a*this.d-this.b*this.c};n.split=function(){var b={};b.dx=this.e;b.dy=this.f;var d=[[this.a,this.c],[this.b,this.d] ];b.scalex=f.sqrt(k(d[0]));p(d[0]);b.shear=d[0][0]*d[1][0]+d[0][1]*d[1][1];d[1]=[d[1][0]-d[0][0]*b.shear,d[1][1]-d[0][1]*b.shear];b.scaley=f.sqrt(k(d[1]));
p(d[1]);b.shear/=b.scaley;0>this.determinant()&&(b.scalex=-b.scalex);var e=-d[0][1],d=d[1][1];0>d?(b.rotate=a.deg(f.acos(d)),0>e&&(b.rotate=360-b.rotate)):b.rotate=a.deg(f.asin(e));b.isSimple=!+b.shear.toFixed(9)&&(b.scalex.toFixed(9)==b.scaley.toFixed(9)||!b.rotate);b.isSuperSimple=!+b.shear.toFixed(9)&&b.scalex.toFixed(9)==b.scaley.toFixed(9)&&!b.rotate;b.noRotation=!+b.shear.toFixed(9)&&!b.rotate;return b};n.toTransformString=function(a){a=a||this.split();if(+a.shear.toFixed(9))return"m"+[this.get(0),
this.get(1),this.get(2),this.get(3),this.get(4),this.get(5)];a.scalex=+a.scalex.toFixed(4);a.scaley=+a.scaley.toFixed(4);a.rotate=+a.rotate.toFixed(4);return(a.dx||a.dy?"t"+[+a.dx.toFixed(4),+a.dy.toFixed(4)]:"")+(1!=a.scalex||1!=a.scaley?"s"+[a.scalex,a.scaley,0,0]:"")+(a.rotate?"r"+[+a.rotate.toFixed(4),0,0]:"")}})(w.prototype);a.Matrix=w;a.matrix=function(a,d,f,b,k,e){return new w(a,d,f,b,k,e)}});C.plugin(function(a,v,y,M,A){function w(h){return function(d){k.stop();d instanceof A&&1==d.node.childNodes.length&&
("radialGradient"==d.node.firstChild.tagName||"linearGradient"==d.node.firstChild.tagName||"pattern"==d.node.firstChild.tagName)&&(d=d.node.firstChild,b(this).appendChild(d),d=u(d));if(d instanceof v)if("radialGradient"==d.type||"linearGradient"==d.type||"pattern"==d.type){d.node.id||e(d.node,{id:d.id});var f=l(d.node.id)}else f=d.attr(h);else f=a.color(d),f.error?(f=a(b(this).ownerSVGElement).gradient(d))?(f.node.id||e(f.node,{id:f.id}),f=l(f.node.id)):f=d:f=r(f);d={};d[h]=f;e(this.node,d);this.node.style[h]=
x}}function z(a){k.stop();a==+a&&(a+="px");this.node.style.fontSize=a}function d(a){var b=[];a=a.childNodes;for(var e=0,f=a.length;e<f;e++){var l=a[e];3==l.nodeType&&b.push(l.nodeValue);"tspan"==l.tagName&&(1==l.childNodes.length&&3==l.firstChild.nodeType?b.push(l.firstChild.nodeValue):b.push(d(l)))}return b}function f(){k.stop();return this.node.style.fontSize}var n=a._.make,u=a._.wrap,p=a.is,b=a._.getSomeDefs,q=/^url\(#?([^)]+)\)$/,e=a._.$,l=a.url,r=String,s=a._.separator,x="";k.on("snap.util.attr.mask",
function(a){if(a instanceof v||a instanceof A){k.stop();a instanceof A&&1==a.node.childNodes.length&&(a=a.node.firstChild,b(this).appendChild(a),a=u(a));if("mask"==a.type)var d=a;else d=n("mask",b(this)),d.node.appendChild(a.node);!d.node.id&&e(d.node,{id:d.id});e(this.node,{mask:l(d.id)})}});(function(a){k.on("snap.util.attr.clip",a);k.on("snap.util.attr.clip-path",a);k.on("snap.util.attr.clipPath",a)})(function(a){if(a instanceof v||a instanceof A){k.stop();if("clipPath"==a.type)var d=a;else d=
n("clipPath",b(this)),d.node.appendChild(a.node),!d.node.id&&e(d.node,{id:d.id});e(this.node,{"clip-path":l(d.id)})}});k.on("snap.util.attr.fill",w("fill"));k.on("snap.util.attr.stroke",w("stroke"));var G=/^([lr])(?:\(([^)]*)\))?(.*)$/i;k.on("snap.util.grad.parse",function(a){a=r(a);var b=a.match(G);if(!b)return null;a=b[1];var e=b[2],b=b[3],e=e.split(/\s*,\s*/).map(function(a){return+a==a?+a:a});1==e.length&&0==e[0]&&(e=[]);b=b.split("-");b=b.map(function(a){a=a.split(":");var b={color:a[0]};a[1]&&
(b.offset=parseFloat(a[1]));return b});return{type:a,params:e,stops:b}});k.on("snap.util.attr.d",function(b){k.stop();p(b,"array")&&p(b[0],"array")&&(b=a.path.toString.call(b));b=r(b);b.match(/[ruo]/i)&&(b=a.path.toAbsolute(b));e(this.node,{d:b})})(-1);k.on("snap.util.attr.#text",function(a){k.stop();a=r(a);for(a=M.doc.createTextNode(a);this.node.firstChild;)this.node.removeChild(this.node.firstChild);this.node.appendChild(a)})(-1);k.on("snap.util.attr.path",function(a){k.stop();this.attr({d:a})})(-1);
k.on("snap.util.attr.class",function(a){k.stop();this.node.className.baseVal=a})(-1);k.on("snap.util.attr.viewBox",function(a){a=p(a,"object")&&"x"in a?[a.x,a.y,a.width,a.height].join(" "):p(a,"array")?a.join(" "):a;e(this.node,{viewBox:a});k.stop()})(-1);k.on("snap.util.attr.transform",function(a){this.transform(a);k.stop()})(-1);k.on("snap.util.attr.r",function(a){"rect"==this.type&&(k.stop(),e(this.node,{rx:a,ry:a}))})(-1);k.on("snap.util.attr.textpath",function(a){k.stop();if("text"==this.type){var d,
f;if(!a&&this.textPath){for(a=this.textPath;a.node.firstChild;)this.node.appendChild(a.node.firstChild);a.remove();delete this.textPath}else if(p(a,"string")?(d=b(this),a=u(d.parentNode).path(a),d.appendChild(a.node),d=a.id,a.attr({id:d})):(a=u(a),a instanceof v&&(d=a.attr("id"),d||(d=a.id,a.attr({id:d})))),d)if(a=this.textPath,f=this.node,a)a.attr({"xlink:href":"#"+d});else{for(a=e("textPath",{"xlink:href":"#"+d});f.firstChild;)a.appendChild(f.firstChild);f.appendChild(a);this.textPath=u(a)}}})(-1);
k.on("snap.util.attr.text",function(a){if("text"==this.type){for(var b=this.node,d=function(a){var b=e("tspan");if(p(a,"array"))for(var f=0;f<a.length;f++)b.appendChild(d(a[f]));else b.appendChild(M.doc.createTextNode(a));b.normalize&&b.normalize();return b};b.firstChild;)b.removeChild(b.firstChild);for(a=d(a);a.firstChild;)b.appendChild(a.firstChild)}k.stop()})(-1);k.on("snap.util.attr.fontSize",z)(-1);k.on("snap.util.attr.font-size",z)(-1);k.on("snap.util.getattr.transform",function(){k.stop();
return this.transform()})(-1);k.on("snap.util.getattr.textpath",function(){k.stop();return this.textPath})(-1);(function(){function b(d){return function(){k.stop();var b=M.doc.defaultView.getComputedStyle(this.node,null).getPropertyValue("marker-"+d);return"none"==b?b:a(M.doc.getElementById(b.match(q)[1]))}}function d(a){return function(b){k.stop();var d="marker"+a.charAt(0).toUpperCase()+a.substring(1);if(""==b||!b)this.node.style[d]="none";else if("marker"==b.type){var f=b.node.id;f||e(b.node,{id:b.id});
this.node.style[d]=l(f)}}}k.on("snap.util.getattr.marker-end",b("end"))(-1);k.on("snap.util.getattr.markerEnd",b("end"))(-1);k.on("snap.util.getattr.marker-start",b("start"))(-1);k.on("snap.util.getattr.markerStart",b("start"))(-1);k.on("snap.util.getattr.marker-mid",b("mid"))(-1);k.on("snap.util.getattr.markerMid",b("mid"))(-1);k.on("snap.util.attr.marker-end",d("end"))(-1);k.on("snap.util.attr.markerEnd",d("end"))(-1);k.on("snap.util.attr.marker-start",d("start"))(-1);k.on("snap.util.attr.markerStart",
d("start"))(-1);k.on("snap.util.attr.marker-mid",d("mid"))(-1);k.on("snap.util.attr.markerMid",d("mid"))(-1)})();k.on("snap.util.getattr.r",function(){if("rect"==this.type&&e(this.node,"rx")==e(this.node,"ry"))return k.stop(),e(this.node,"rx")})(-1);k.on("snap.util.getattr.text",function(){if("text"==this.type||"tspan"==this.type){k.stop();var a=d(this.node);return 1==a.length?a[0]:a}})(-1);k.on("snap.util.getattr.#text",function(){return this.node.textContent})(-1);k.on("snap.util.getattr.viewBox",
function(){k.stop();var b=e(this.node,"viewBox");if(b)return b=b.split(s),a._.box(+b[0],+b[1],+b[2],+b[3])})(-1);k.on("snap.util.getattr.points",function(){var a=e(this.node,"points");k.stop();if(a)return a.split(s)})(-1);k.on("snap.util.getattr.path",function(){var a=e(this.node,"d");k.stop();return a})(-1);k.on("snap.util.getattr.class",function(){return this.node.className.baseVal})(-1);k.on("snap.util.getattr.fontSize",f)(-1);k.on("snap.util.getattr.font-size",f)(-1)});C.plugin(function(a,v,y,
M,A){function w(a){return a}function z(a){return function(b){return+b.toFixed(3)+a}}var d={"+":function(a,b){return a+b},"-":function(a,b){return a-b},"/":function(a,b){return a/b},"*":function(a,b){return a*b}},f=String,n=/[a-z]+$/i,u=/^\s*([+\-\/*])\s*=\s*([\d.eE+\-]+)\s*([^\d\s]+)?\s*$/;k.on("snap.util.attr",function(a){if(a=f(a).match(u)){var b=k.nt(),b=b.substring(b.lastIndexOf(".")+1),q=this.attr(b),e={};k.stop();var l=a[3]||"",r=q.match(n),s=d[a[1] ];r&&r==l?a=s(parseFloat(q),+a[2]):(q=this.asPX(b),
a=s(this.asPX(b),this.asPX(b,a[2]+l)));isNaN(q)||isNaN(a)||(e[b]=a,this.attr(e))}})(-10);k.on("snap.util.equal",function(a,b){var q=f(this.attr(a)||""),e=f(b).match(u);if(e){k.stop();var l=e[3]||"",r=q.match(n),s=d[e[1] ];if(r&&r==l)return{from:parseFloat(q),to:s(parseFloat(q),+e[2]),f:z(r)};q=this.asPX(a);return{from:q,to:s(q,this.asPX(a,e[2]+l)),f:w}}})(-10)});C.plugin(function(a,v,y,M,A){var w=y.prototype,z=a.is;w.rect=function(a,d,k,p,b,q){var e;null==q&&(q=b);z(a,"object")&&"[object Object]"==
a?e=a:null!=a&&(e={x:a,y:d,width:k,height:p},null!=b&&(e.rx=b,e.ry=q));return this.el("rect",e)};w.circle=function(a,d,k){var p;z(a,"object")&&"[object Object]"==a?p=a:null!=a&&(p={cx:a,cy:d,r:k});return this.el("circle",p)};var d=function(){function a(){this.parentNode.removeChild(this)}return function(d,k){var p=M.doc.createElement("img"),b=M.doc.body;p.style.cssText="position:absolute;left:-9999em;top:-9999em";p.onload=function(){k.call(p);p.onload=p.onerror=null;b.removeChild(p)};p.onerror=a;
b.appendChild(p);p.src=d}}();w.image=function(f,n,k,p,b){var q=this.el("image");if(z(f,"object")&&"src"in f)q.attr(f);else if(null!=f){var e={"xlink:href":f,preserveAspectRatio:"none"};null!=n&&null!=k&&(e.x=n,e.y=k);null!=p&&null!=b?(e.width=p,e.height=b):d(f,function(){a._.$(q.node,{width:this.offsetWidth,height:this.offsetHeight})});a._.$(q.node,e)}return q};w.ellipse=function(a,d,k,p){var b;z(a,"object")&&"[object Object]"==a?b=a:null!=a&&(b={cx:a,cy:d,rx:k,ry:p});return this.el("ellipse",b)};
w.path=function(a){var d;z(a,"object")&&!z(a,"array")?d=a:a&&(d={d:a});return this.el("path",d)};w.group=w.g=function(a){var d=this.el("g");1==arguments.length&&a&&!a.type?d.attr(a):arguments.length&&d.add(Array.prototype.slice.call(arguments,0));return d};w.svg=function(a,d,k,p,b,q,e,l){var r={};z(a,"object")&&null==d?r=a:(null!=a&&(r.x=a),null!=d&&(r.y=d),null!=k&&(r.width=k),null!=p&&(r.height=p),null!=b&&null!=q&&null!=e&&null!=l&&(r.viewBox=[b,q,e,l]));return this.el("svg",r)};w.mask=function(a){var d=
this.el("mask");1==arguments.length&&a&&!a.type?d.attr(a):arguments.length&&d.add(Array.prototype.slice.call(arguments,0));return d};w.ptrn=function(a,d,k,p,b,q,e,l){if(z(a,"object"))var r=a;else arguments.length?(r={},null!=a&&(r.x=a),null!=d&&(r.y=d),null!=k&&(r.width=k),null!=p&&(r.height=p),null!=b&&null!=q&&null!=e&&null!=l&&(r.viewBox=[b,q,e,l])):r={patternUnits:"userSpaceOnUse"};return this.el("pattern",r)};w.use=function(a){return null!=a?(make("use",this.node),a instanceof v&&(a.attr("id")||
a.attr({id:ID()}),a=a.attr("id")),this.el("use",{"xlink:href":a})):v.prototype.use.call(this)};w.text=function(a,d,k){var p={};z(a,"object")?p=a:null!=a&&(p={x:a,y:d,text:k||""});return this.el("text",p)};w.line=function(a,d,k,p){var b={};z(a,"object")?b=a:null!=a&&(b={x1:a,x2:k,y1:d,y2:p});return this.el("line",b)};w.polyline=function(a){1<arguments.length&&(a=Array.prototype.slice.call(arguments,0));var d={};z(a,"object")&&!z(a,"array")?d=a:null!=a&&(d={points:a});return this.el("polyline",d)};
w.polygon=function(a){1<arguments.length&&(a=Array.prototype.slice.call(arguments,0));var d={};z(a,"object")&&!z(a,"array")?d=a:null!=a&&(d={points:a});return this.el("polygon",d)};(function(){function d(){return this.selectAll("stop")}function n(b,d){var f=e("stop"),k={offset:+d+"%"};b=a.color(b);k["stop-color"]=b.hex;1>b.opacity&&(k["stop-opacity"]=b.opacity);e(f,k);this.node.appendChild(f);return this}function u(){if("linearGradient"==this.type){var b=e(this.node,"x1")||0,d=e(this.node,"x2")||
1,f=e(this.node,"y1")||0,k=e(this.node,"y2")||0;return a._.box(b,f,math.abs(d-b),math.abs(k-f))}b=this.node.r||0;return a._.box((this.node.cx||0.5)-b,(this.node.cy||0.5)-b,2*b,2*b)}function p(a,d){function f(a,b){for(var d=(b-u)/(a-w),e=w;e<a;e++)h[e].offset=+(+u+d*(e-w)).toFixed(2);w=a;u=b}var n=k("snap.util.grad.parse",null,d).firstDefined(),p;if(!n)return null;n.params.unshift(a);p="l"==n.type.toLowerCase()?b.apply(0,n.params):q.apply(0,n.params);n.type!=n.type.toLowerCase()&&e(p.node,{gradientUnits:"userSpaceOnUse"});
var h=n.stops,n=h.length,u=0,w=0;n--;for(var v=0;v<n;v++)"offset"in h[v]&&f(v,h[v].offset);h[n].offset=h[n].offset||100;f(n,h[n].offset);for(v=0;v<=n;v++){var y=h[v];p.addStop(y.color,y.offset)}return p}function b(b,k,p,q,w){b=a._.make("linearGradient",b);b.stops=d;b.addStop=n;b.getBBox=u;null!=k&&e(b.node,{x1:k,y1:p,x2:q,y2:w});return b}function q(b,k,p,q,w,h){b=a._.make("radialGradient",b);b.stops=d;b.addStop=n;b.getBBox=u;null!=k&&e(b.node,{cx:k,cy:p,r:q});null!=w&&null!=h&&e(b.node,{fx:w,fy:h});
return b}var e=a._.$;w.gradient=function(a){return p(this.defs,a)};w.gradientLinear=function(a,d,e,f){return b(this.defs,a,d,e,f)};w.gradientRadial=function(a,b,d,e,f){return q(this.defs,a,b,d,e,f)};w.toString=function(){var b=this.node.ownerDocument,d=b.createDocumentFragment(),b=b.createElement("div"),e=this.node.cloneNode(!0);d.appendChild(b);b.appendChild(e);a._.$(e,{xmlns:"http://www.w3.org/2000/svg"});b=b.innerHTML;d.removeChild(d.firstChild);return b};w.clear=function(){for(var a=this.node.firstChild,
b;a;)b=a.nextSibling,"defs"!=a.tagName?a.parentNode.removeChild(a):w.clear.call({node:a}),a=b}})()});C.plugin(function(a,k,y,M){function A(a){var b=A.ps=A.ps||{};b[a]?b[a].sleep=100:b[a]={sleep:100};setTimeout(function(){for(var d in b)b[L](d)&&d!=a&&(b[d].sleep--,!b[d].sleep&&delete b[d])});return b[a]}function w(a,b,d,e){null==a&&(a=b=d=e=0);null==b&&(b=a.y,d=a.width,e=a.height,a=a.x);return{x:a,y:b,width:d,w:d,height:e,h:e,x2:a+d,y2:b+e,cx:a+d/2,cy:b+e/2,r1:F.min(d,e)/2,r2:F.max(d,e)/2,r0:F.sqrt(d*
d+e*e)/2,path:s(a,b,d,e),vb:[a,b,d,e].join(" ")}}function z(){return this.join(",").replace(N,"$1")}function d(a){a=C(a);a.toString=z;return a}function f(a,b,d,h,f,k,l,n,p){if(null==p)return e(a,b,d,h,f,k,l,n);if(0>p||e(a,b,d,h,f,k,l,n)<p)p=void 0;else{var q=0.5,O=1-q,s;for(s=e(a,b,d,h,f,k,l,n,O);0.01<Z(s-p);)q/=2,O+=(s<p?1:-1)*q,s=e(a,b,d,h,f,k,l,n,O);p=O}return u(a,b,d,h,f,k,l,n,p)}function n(b,d){function e(a){return+(+a).toFixed(3)}return a._.cacher(function(a,h,l){a instanceof k&&(a=a.attr("d"));
a=I(a);for(var n,p,D,q,O="",s={},c=0,t=0,r=a.length;t<r;t++){D=a[t];if("M"==D[0])n=+D[1],p=+D[2];else{q=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6]);if(c+q>h){if(d&&!s.start){n=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6],h-c);O+=["C"+e(n.start.x),e(n.start.y),e(n.m.x),e(n.m.y),e(n.x),e(n.y)];if(l)return O;s.start=O;O=["M"+e(n.x),e(n.y)+"C"+e(n.n.x),e(n.n.y),e(n.end.x),e(n.end.y),e(D[5]),e(D[6])].join();c+=q;n=+D[5];p=+D[6];continue}if(!b&&!d)return n=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6],h-c)}c+=q;n=+D[5];p=+D[6]}O+=
D.shift()+D}s.end=O;return n=b?c:d?s:u(n,p,D[0],D[1],D[2],D[3],D[4],D[5],1)},null,a._.clone)}function u(a,b,d,e,h,f,k,l,n){var p=1-n,q=ma(p,3),s=ma(p,2),c=n*n,t=c*n,r=q*a+3*s*n*d+3*p*n*n*h+t*k,q=q*b+3*s*n*e+3*p*n*n*f+t*l,s=a+2*n*(d-a)+c*(h-2*d+a),t=b+2*n*(e-b)+c*(f-2*e+b),x=d+2*n*(h-d)+c*(k-2*h+d),c=e+2*n*(f-e)+c*(l-2*f+e);a=p*a+n*d;b=p*b+n*e;h=p*h+n*k;f=p*f+n*l;l=90-180*F.atan2(s-x,t-c)/S;return{x:r,y:q,m:{x:s,y:t},n:{x:x,y:c},start:{x:a,y:b},end:{x:h,y:f},alpha:l}}function p(b,d,e,h,f,n,k,l){a.is(b,
"array")||(b=[b,d,e,h,f,n,k,l]);b=U.apply(null,b);return w(b.min.x,b.min.y,b.max.x-b.min.x,b.max.y-b.min.y)}function b(a,b,d){return b>=a.x&&b<=a.x+a.width&&d>=a.y&&d<=a.y+a.height}function q(a,d){a=w(a);d=w(d);return b(d,a.x,a.y)||b(d,a.x2,a.y)||b(d,a.x,a.y2)||b(d,a.x2,a.y2)||b(a,d.x,d.y)||b(a,d.x2,d.y)||b(a,d.x,d.y2)||b(a,d.x2,d.y2)||(a.x<d.x2&&a.x>d.x||d.x<a.x2&&d.x>a.x)&&(a.y<d.y2&&a.y>d.y||d.y<a.y2&&d.y>a.y)}function e(a,b,d,e,h,f,n,k,l){null==l&&(l=1);l=(1<l?1:0>l?0:l)/2;for(var p=[-0.1252,
0.1252,-0.3678,0.3678,-0.5873,0.5873,-0.7699,0.7699,-0.9041,0.9041,-0.9816,0.9816],q=[0.2491,0.2491,0.2335,0.2335,0.2032,0.2032,0.1601,0.1601,0.1069,0.1069,0.0472,0.0472],s=0,c=0;12>c;c++)var t=l*p[c]+l,r=t*(t*(-3*a+9*d-9*h+3*n)+6*a-12*d+6*h)-3*a+3*d,t=t*(t*(-3*b+9*e-9*f+3*k)+6*b-12*e+6*f)-3*b+3*e,s=s+q[c]*F.sqrt(r*r+t*t);return l*s}function l(a,b,d){a=I(a);b=I(b);for(var h,f,l,n,k,s,r,O,x,c,t=d?0:[],w=0,v=a.length;w<v;w++)if(x=a[w],"M"==x[0])h=k=x[1],f=s=x[2];else{"C"==x[0]?(x=[h,f].concat(x.slice(1)),
h=x[6],f=x[7]):(x=[h,f,h,f,k,s,k,s],h=k,f=s);for(var G=0,y=b.length;G<y;G++)if(c=b[G],"M"==c[0])l=r=c[1],n=O=c[2];else{"C"==c[0]?(c=[l,n].concat(c.slice(1)),l=c[6],n=c[7]):(c=[l,n,l,n,r,O,r,O],l=r,n=O);var z;var K=x,B=c;z=d;var H=p(K),J=p(B);if(q(H,J)){for(var H=e.apply(0,K),J=e.apply(0,B),H=~~(H/8),J=~~(J/8),U=[],A=[],F={},M=z?0:[],P=0;P<H+1;P++){var C=u.apply(0,K.concat(P/H));U.push({x:C.x,y:C.y,t:P/H})}for(P=0;P<J+1;P++)C=u.apply(0,B.concat(P/J)),A.push({x:C.x,y:C.y,t:P/J});for(P=0;P<H;P++)for(K=
0;K<J;K++){var Q=U[P],L=U[P+1],B=A[K],C=A[K+1],N=0.001>Z(L.x-Q.x)?"y":"x",S=0.001>Z(C.x-B.x)?"y":"x",R;R=Q.x;var Y=Q.y,V=L.x,ea=L.y,fa=B.x,ga=B.y,ha=C.x,ia=C.y;if(W(R,V)<X(fa,ha)||X(R,V)>W(fa,ha)||W(Y,ea)<X(ga,ia)||X(Y,ea)>W(ga,ia))R=void 0;else{var $=(R*ea-Y*V)*(fa-ha)-(R-V)*(fa*ia-ga*ha),aa=(R*ea-Y*V)*(ga-ia)-(Y-ea)*(fa*ia-ga*ha),ja=(R-V)*(ga-ia)-(Y-ea)*(fa-ha);if(ja){var $=$/ja,aa=aa/ja,ja=+$.toFixed(2),ba=+aa.toFixed(2);R=ja<+X(R,V).toFixed(2)||ja>+W(R,V).toFixed(2)||ja<+X(fa,ha).toFixed(2)||
ja>+W(fa,ha).toFixed(2)||ba<+X(Y,ea).toFixed(2)||ba>+W(Y,ea).toFixed(2)||ba<+X(ga,ia).toFixed(2)||ba>+W(ga,ia).toFixed(2)?void 0:{x:$,y:aa}}else R=void 0}R&&F[R.x.toFixed(4)]!=R.y.toFixed(4)&&(F[R.x.toFixed(4)]=R.y.toFixed(4),Q=Q.t+Z((R[N]-Q[N])/(L[N]-Q[N]))*(L.t-Q.t),B=B.t+Z((R[S]-B[S])/(C[S]-B[S]))*(C.t-B.t),0<=Q&&1>=Q&&0<=B&&1>=B&&(z?M++:M.push({x:R.x,y:R.y,t1:Q,t2:B})))}z=M}else z=z?0:[];if(d)t+=z;else{H=0;for(J=z.length;H<J;H++)z[H].segment1=w,z[H].segment2=G,z[H].bez1=x,z[H].bez2=c;t=t.concat(z)}}}return t}
function r(a){var b=A(a);if(b.bbox)return C(b.bbox);if(!a)return w();a=I(a);for(var d=0,e=0,h=[],f=[],l,n=0,k=a.length;n<k;n++)l=a[n],"M"==l[0]?(d=l[1],e=l[2],h.push(d),f.push(e)):(d=U(d,e,l[1],l[2],l[3],l[4],l[5],l[6]),h=h.concat(d.min.x,d.max.x),f=f.concat(d.min.y,d.max.y),d=l[5],e=l[6]);a=X.apply(0,h);l=X.apply(0,f);h=W.apply(0,h);f=W.apply(0,f);f=w(a,l,h-a,f-l);b.bbox=C(f);return f}function s(a,b,d,e,h){if(h)return[["M",+a+ +h,b],["l",d-2*h,0],["a",h,h,0,0,1,h,h],["l",0,e-2*h],["a",h,h,0,0,1,
-h,h],["l",2*h-d,0],["a",h,h,0,0,1,-h,-h],["l",0,2*h-e],["a",h,h,0,0,1,h,-h],["z"] ];a=[["M",a,b],["l",d,0],["l",0,e],["l",-d,0],["z"] ];a.toString=z;return a}function x(a,b,d,e,h){null==h&&null==e&&(e=d);a=+a;b=+b;d=+d;e=+e;if(null!=h){var f=Math.PI/180,l=a+d*Math.cos(-e*f);a+=d*Math.cos(-h*f);var n=b+d*Math.sin(-e*f);b+=d*Math.sin(-h*f);d=[["M",l,n],["A",d,d,0,+(180<h-e),0,a,b] ]}else d=[["M",a,b],["m",0,-e],["a",d,e,0,1,1,0,2*e],["a",d,e,0,1,1,0,-2*e],["z"] ];d.toString=z;return d}function G(b){var e=
A(b);if(e.abs)return d(e.abs);Q(b,"array")&&Q(b&&b[0],"array")||(b=a.parsePathString(b));if(!b||!b.length)return[["M",0,0] ];var h=[],f=0,l=0,n=0,k=0,p=0;"M"==b[0][0]&&(f=+b[0][1],l=+b[0][2],n=f,k=l,p++,h[0]=["M",f,l]);for(var q=3==b.length&&"M"==b[0][0]&&"R"==b[1][0].toUpperCase()&&"Z"==b[2][0].toUpperCase(),s,r,w=p,c=b.length;w<c;w++){h.push(s=[]);r=b[w];p=r[0];if(p!=p.toUpperCase())switch(s[0]=p.toUpperCase(),s[0]){case "A":s[1]=r[1];s[2]=r[2];s[3]=r[3];s[4]=r[4];s[5]=r[5];s[6]=+r[6]+f;s[7]=+r[7]+
l;break;case "V":s[1]=+r[1]+l;break;case "H":s[1]=+r[1]+f;break;case "R":for(var t=[f,l].concat(r.slice(1)),u=2,v=t.length;u<v;u++)t[u]=+t[u]+f,t[++u]=+t[u]+l;h.pop();h=h.concat(P(t,q));break;case "O":h.pop();t=x(f,l,r[1],r[2]);t.push(t[0]);h=h.concat(t);break;case "U":h.pop();h=h.concat(x(f,l,r[1],r[2],r[3]));s=["U"].concat(h[h.length-1].slice(-2));break;case "M":n=+r[1]+f,k=+r[2]+l;default:for(u=1,v=r.length;u<v;u++)s[u]=+r[u]+(u%2?f:l)}else if("R"==p)t=[f,l].concat(r.slice(1)),h.pop(),h=h.concat(P(t,
q)),s=["R"].concat(r.slice(-2));else if("O"==p)h.pop(),t=x(f,l,r[1],r[2]),t.push(t[0]),h=h.concat(t);else if("U"==p)h.pop(),h=h.concat(x(f,l,r[1],r[2],r[3])),s=["U"].concat(h[h.length-1].slice(-2));else for(t=0,u=r.length;t<u;t++)s[t]=r[t];p=p.toUpperCase();if("O"!=p)switch(s[0]){case "Z":f=+n;l=+k;break;case "H":f=s[1];break;case "V":l=s[1];break;case "M":n=s[s.length-2],k=s[s.length-1];default:f=s[s.length-2],l=s[s.length-1]}}h.toString=z;e.abs=d(h);return h}function h(a,b,d,e){return[a,b,d,e,d,
e]}function J(a,b,d,e,h,f){var l=1/3,n=2/3;return[l*a+n*d,l*b+n*e,l*h+n*d,l*f+n*e,h,f]}function K(b,d,e,h,f,l,n,k,p,s){var r=120*S/180,q=S/180*(+f||0),c=[],t,x=a._.cacher(function(a,b,c){var d=a*F.cos(c)-b*F.sin(c);a=a*F.sin(c)+b*F.cos(c);return{x:d,y:a}});if(s)v=s[0],t=s[1],l=s[2],u=s[3];else{t=x(b,d,-q);b=t.x;d=t.y;t=x(k,p,-q);k=t.x;p=t.y;F.cos(S/180*f);F.sin(S/180*f);t=(b-k)/2;v=(d-p)/2;u=t*t/(e*e)+v*v/(h*h);1<u&&(u=F.sqrt(u),e*=u,h*=u);var u=e*e,w=h*h,u=(l==n?-1:1)*F.sqrt(Z((u*w-u*v*v-w*t*t)/
(u*v*v+w*t*t)));l=u*e*v/h+(b+k)/2;var u=u*-h*t/e+(d+p)/2,v=F.asin(((d-u)/h).toFixed(9));t=F.asin(((p-u)/h).toFixed(9));v=b<l?S-v:v;t=k<l?S-t:t;0>v&&(v=2*S+v);0>t&&(t=2*S+t);n&&v>t&&(v-=2*S);!n&&t>v&&(t-=2*S)}if(Z(t-v)>r){var c=t,w=k,G=p;t=v+r*(n&&t>v?1:-1);k=l+e*F.cos(t);p=u+h*F.sin(t);c=K(k,p,e,h,f,0,n,w,G,[t,c,l,u])}l=t-v;f=F.cos(v);r=F.sin(v);n=F.cos(t);t=F.sin(t);l=F.tan(l/4);e=4/3*e*l;l*=4/3*h;h=[b,d];b=[b+e*r,d-l*f];d=[k+e*t,p-l*n];k=[k,p];b[0]=2*h[0]-b[0];b[1]=2*h[1]-b[1];if(s)return[b,d,k].concat(c);
c=[b,d,k].concat(c).join().split(",");s=[];k=0;for(p=c.length;k<p;k++)s[k]=k%2?x(c[k-1],c[k],q).y:x(c[k],c[k+1],q).x;return s}function U(a,b,d,e,h,f,l,k){for(var n=[],p=[[],[] ],s,r,c,t,q=0;2>q;++q)0==q?(r=6*a-12*d+6*h,s=-3*a+9*d-9*h+3*l,c=3*d-3*a):(r=6*b-12*e+6*f,s=-3*b+9*e-9*f+3*k,c=3*e-3*b),1E-12>Z(s)?1E-12>Z(r)||(s=-c/r,0<s&&1>s&&n.push(s)):(t=r*r-4*c*s,c=F.sqrt(t),0>t||(t=(-r+c)/(2*s),0<t&&1>t&&n.push(t),s=(-r-c)/(2*s),0<s&&1>s&&n.push(s)));for(r=q=n.length;q--;)s=n[q],c=1-s,p[0][q]=c*c*c*a+3*
c*c*s*d+3*c*s*s*h+s*s*s*l,p[1][q]=c*c*c*b+3*c*c*s*e+3*c*s*s*f+s*s*s*k;p[0][r]=a;p[1][r]=b;p[0][r+1]=l;p[1][r+1]=k;p[0].length=p[1].length=r+2;return{min:{x:X.apply(0,p[0]),y:X.apply(0,p[1])},max:{x:W.apply(0,p[0]),y:W.apply(0,p[1])}}}function I(a,b){var e=!b&&A(a);if(!b&&e.curve)return d(e.curve);var f=G(a),l=b&&G(b),n={x:0,y:0,bx:0,by:0,X:0,Y:0,qx:null,qy:null},k={x:0,y:0,bx:0,by:0,X:0,Y:0,qx:null,qy:null},p=function(a,b,c){if(!a)return["C",b.x,b.y,b.x,b.y,b.x,b.y];a[0]in{T:1,Q:1}||(b.qx=b.qy=null);
switch(a[0]){case "M":b.X=a[1];b.Y=a[2];break;case "A":a=["C"].concat(K.apply(0,[b.x,b.y].concat(a.slice(1))));break;case "S":"C"==c||"S"==c?(c=2*b.x-b.bx,b=2*b.y-b.by):(c=b.x,b=b.y);a=["C",c,b].concat(a.slice(1));break;case "T":"Q"==c||"T"==c?(b.qx=2*b.x-b.qx,b.qy=2*b.y-b.qy):(b.qx=b.x,b.qy=b.y);a=["C"].concat(J(b.x,b.y,b.qx,b.qy,a[1],a[2]));break;case "Q":b.qx=a[1];b.qy=a[2];a=["C"].concat(J(b.x,b.y,a[1],a[2],a[3],a[4]));break;case "L":a=["C"].concat(h(b.x,b.y,a[1],a[2]));break;case "H":a=["C"].concat(h(b.x,
b.y,a[1],b.y));break;case "V":a=["C"].concat(h(b.x,b.y,b.x,a[1]));break;case "Z":a=["C"].concat(h(b.x,b.y,b.X,b.Y))}return a},s=function(a,b){if(7<a[b].length){a[b].shift();for(var c=a[b];c.length;)q[b]="A",l&&(u[b]="A"),a.splice(b++,0,["C"].concat(c.splice(0,6)));a.splice(b,1);v=W(f.length,l&&l.length||0)}},r=function(a,b,c,d,e){a&&b&&"M"==a[e][0]&&"M"!=b[e][0]&&(b.splice(e,0,["M",d.x,d.y]),c.bx=0,c.by=0,c.x=a[e][1],c.y=a[e][2],v=W(f.length,l&&l.length||0))},q=[],u=[],c="",t="",x=0,v=W(f.length,
l&&l.length||0);for(;x<v;x++){f[x]&&(c=f[x][0]);"C"!=c&&(q[x]=c,x&&(t=q[x-1]));f[x]=p(f[x],n,t);"A"!=q[x]&&"C"==c&&(q[x]="C");s(f,x);l&&(l[x]&&(c=l[x][0]),"C"!=c&&(u[x]=c,x&&(t=u[x-1])),l[x]=p(l[x],k,t),"A"!=u[x]&&"C"==c&&(u[x]="C"),s(l,x));r(f,l,n,k,x);r(l,f,k,n,x);var w=f[x],z=l&&l[x],y=w.length,U=l&&z.length;n.x=w[y-2];n.y=w[y-1];n.bx=$(w[y-4])||n.x;n.by=$(w[y-3])||n.y;k.bx=l&&($(z[U-4])||k.x);k.by=l&&($(z[U-3])||k.y);k.x=l&&z[U-2];k.y=l&&z[U-1]}l||(e.curve=d(f));return l?[f,l]:f}function P(a,
b){for(var d=[],e=0,h=a.length;h-2*!b>e;e+=2){var f=[{x:+a[e-2],y:+a[e-1]},{x:+a[e],y:+a[e+1]},{x:+a[e+2],y:+a[e+3]},{x:+a[e+4],y:+a[e+5]}];b?e?h-4==e?f[3]={x:+a[0],y:+a[1]}:h-2==e&&(f[2]={x:+a[0],y:+a[1]},f[3]={x:+a[2],y:+a[3]}):f[0]={x:+a[h-2],y:+a[h-1]}:h-4==e?f[3]=f[2]:e||(f[0]={x:+a[e],y:+a[e+1]});d.push(["C",(-f[0].x+6*f[1].x+f[2].x)/6,(-f[0].y+6*f[1].y+f[2].y)/6,(f[1].x+6*f[2].x-f[3].x)/6,(f[1].y+6*f[2].y-f[3].y)/6,f[2].x,f[2].y])}return d}y=k.prototype;var Q=a.is,C=a._.clone,L="hasOwnProperty",
N=/,?([a-z]),?/gi,$=parseFloat,F=Math,S=F.PI,X=F.min,W=F.max,ma=F.pow,Z=F.abs;M=n(1);var na=n(),ba=n(0,1),V=a._unit2px;a.path=A;a.path.getTotalLength=M;a.path.getPointAtLength=na;a.path.getSubpath=function(a,b,d){if(1E-6>this.getTotalLength(a)-d)return ba(a,b).end;a=ba(a,d,1);return b?ba(a,b).end:a};y.getTotalLength=function(){if(this.node.getTotalLength)return this.node.getTotalLength()};y.getPointAtLength=function(a){return na(this.attr("d"),a)};y.getSubpath=function(b,d){return a.path.getSubpath(this.attr("d"),
b,d)};a._.box=w;a.path.findDotsAtSegment=u;a.path.bezierBBox=p;a.path.isPointInsideBBox=b;a.path.isBBoxIntersect=q;a.path.intersection=function(a,b){return l(a,b)};a.path.intersectionNumber=function(a,b){return l(a,b,1)};a.path.isPointInside=function(a,d,e){var h=r(a);return b(h,d,e)&&1==l(a,[["M",d,e],["H",h.x2+10] ],1)%2};a.path.getBBox=r;a.path.get={path:function(a){return a.attr("path")},circle:function(a){a=V(a);return x(a.cx,a.cy,a.r)},ellipse:function(a){a=V(a);return x(a.cx||0,a.cy||0,a.rx,
a.ry)},rect:function(a){a=V(a);return s(a.x||0,a.y||0,a.width,a.height,a.rx,a.ry)},image:function(a){a=V(a);return s(a.x||0,a.y||0,a.width,a.height)},line:function(a){return"M"+[a.attr("x1")||0,a.attr("y1")||0,a.attr("x2"),a.attr("y2")]},polyline:function(a){return"M"+a.attr("points")},polygon:function(a){return"M"+a.attr("points")+"z"},deflt:function(a){a=a.node.getBBox();return s(a.x,a.y,a.width,a.height)}};a.path.toRelative=function(b){var e=A(b),h=String.prototype.toLowerCase;if(e.rel)return d(e.rel);
a.is(b,"array")&&a.is(b&&b[0],"array")||(b=a.parsePathString(b));var f=[],l=0,n=0,k=0,p=0,s=0;"M"==b[0][0]&&(l=b[0][1],n=b[0][2],k=l,p=n,s++,f.push(["M",l,n]));for(var r=b.length;s<r;s++){var q=f[s]=[],x=b[s];if(x[0]!=h.call(x[0]))switch(q[0]=h.call(x[0]),q[0]){case "a":q[1]=x[1];q[2]=x[2];q[3]=x[3];q[4]=x[4];q[5]=x[5];q[6]=+(x[6]-l).toFixed(3);q[7]=+(x[7]-n).toFixed(3);break;case "v":q[1]=+(x[1]-n).toFixed(3);break;case "m":k=x[1],p=x[2];default:for(var c=1,t=x.length;c<t;c++)q[c]=+(x[c]-(c%2?l:
n)).toFixed(3)}else for(f[s]=[],"m"==x[0]&&(k=x[1]+l,p=x[2]+n),q=0,c=x.length;q<c;q++)f[s][q]=x[q];x=f[s].length;switch(f[s][0]){case "z":l=k;n=p;break;case "h":l+=+f[s][x-1];break;case "v":n+=+f[s][x-1];break;default:l+=+f[s][x-2],n+=+f[s][x-1]}}f.toString=z;e.rel=d(f);return f};a.path.toAbsolute=G;a.path.toCubic=I;a.path.map=function(a,b){if(!b)return a;var d,e,h,f,l,n,k;a=I(a);h=0;for(l=a.length;h<l;h++)for(k=a[h],f=1,n=k.length;f<n;f+=2)d=b.x(k[f],k[f+1]),e=b.y(k[f],k[f+1]),k[f]=d,k[f+1]=e;return a};
a.path.toString=z;a.path.clone=d});C.plugin(function(a,v,y,C){var A=Math.max,w=Math.min,z=function(a){this.items=[];this.bindings={};this.length=0;this.type="set";if(a)for(var f=0,n=a.length;f<n;f++)a[f]&&(this[this.items.length]=this.items[this.items.length]=a[f],this.length++)};v=z.prototype;v.push=function(){for(var a,f,n=0,k=arguments.length;n<k;n++)if(a=arguments[n])f=this.items.length,this[f]=this.items[f]=a,this.length++;return this};v.pop=function(){this.length&&delete this[this.length--];
return this.items.pop()};v.forEach=function(a,f){for(var n=0,k=this.items.length;n<k&&!1!==a.call(f,this.items[n],n);n++);return this};v.animate=function(d,f,n,u){"function"!=typeof n||n.length||(u=n,n=L.linear);d instanceof a._.Animation&&(u=d.callback,n=d.easing,f=n.dur,d=d.attr);var p=arguments;if(a.is(d,"array")&&a.is(p[p.length-1],"array"))var b=!0;var q,e=function(){q?this.b=q:q=this.b},l=0,r=u&&function(){l++==this.length&&u.call(this)};return this.forEach(function(a,l){k.once("snap.animcreated."+
a.id,e);b?p[l]&&a.animate.apply(a,p[l]):a.animate(d,f,n,r)})};v.remove=function(){for(;this.length;)this.pop().remove();return this};v.bind=function(a,f,k){var u={};if("function"==typeof f)this.bindings[a]=f;else{var p=k||a;this.bindings[a]=function(a){u[p]=a;f.attr(u)}}return this};v.attr=function(a){var f={},k;for(k in a)if(this.bindings[k])this.bindings[k](a[k]);else f[k]=a[k];a=0;for(k=this.items.length;a<k;a++)this.items[a].attr(f);return this};v.clear=function(){for(;this.length;)this.pop()};
v.splice=function(a,f,k){a=0>a?A(this.length+a,0):a;f=A(0,w(this.length-a,f));var u=[],p=[],b=[],q;for(q=2;q<arguments.length;q++)b.push(arguments[q]);for(q=0;q<f;q++)p.push(this[a+q]);for(;q<this.length-a;q++)u.push(this[a+q]);var e=b.length;for(q=0;q<e+u.length;q++)this.items[a+q]=this[a+q]=q<e?b[q]:u[q-e];for(q=this.items.length=this.length-=f-e;this[q];)delete this[q++];return new z(p)};v.exclude=function(a){for(var f=0,k=this.length;f<k;f++)if(this[f]==a)return this.splice(f,1),!0;return!1};
v.insertAfter=function(a){for(var f=this.items.length;f--;)this.items[f].insertAfter(a);return this};v.getBBox=function(){for(var a=[],f=[],k=[],u=[],p=this.items.length;p--;)if(!this.items[p].removed){var b=this.items[p].getBBox();a.push(b.x);f.push(b.y);k.push(b.x+b.width);u.push(b.y+b.height)}a=w.apply(0,a);f=w.apply(0,f);k=A.apply(0,k);u=A.apply(0,u);return{x:a,y:f,x2:k,y2:u,width:k-a,height:u-f,cx:a+(k-a)/2,cy:f+(u-f)/2}};v.clone=function(a){a=new z;for(var f=0,k=this.items.length;f<k;f++)a.push(this.items[f].clone());
return a};v.toString=function(){return"Snap\u2018s set"};v.type="set";a.set=function(){var a=new z;arguments.length&&a.push.apply(a,Array.prototype.slice.call(arguments,0));return a}});C.plugin(function(a,v,y,C){function A(a){var b=a[0];switch(b.toLowerCase()){case "t":return[b,0,0];case "m":return[b,1,0,0,1,0,0];case "r":return 4==a.length?[b,0,a[2],a[3] ]:[b,0];case "s":return 5==a.length?[b,1,1,a[3],a[4] ]:3==a.length?[b,1,1]:[b,1]}}function w(b,d,f){d=q(d).replace(/\.{3}|\u2026/g,b);b=a.parseTransformString(b)||
[];d=a.parseTransformString(d)||[];for(var k=Math.max(b.length,d.length),p=[],v=[],h=0,w,z,y,I;h<k;h++){y=b[h]||A(d[h]);I=d[h]||A(y);if(y[0]!=I[0]||"r"==y[0].toLowerCase()&&(y[2]!=I[2]||y[3]!=I[3])||"s"==y[0].toLowerCase()&&(y[3]!=I[3]||y[4]!=I[4])){b=a._.transform2matrix(b,f());d=a._.transform2matrix(d,f());p=[["m",b.a,b.b,b.c,b.d,b.e,b.f] ];v=[["m",d.a,d.b,d.c,d.d,d.e,d.f] ];break}p[h]=[];v[h]=[];w=0;for(z=Math.max(y.length,I.length);w<z;w++)w in y&&(p[h][w]=y[w]),w in I&&(v[h][w]=I[w])}return{from:u(p),
to:u(v),f:n(p)}}function z(a){return a}function d(a){return function(b){return+b.toFixed(3)+a}}function f(b){return a.rgb(b[0],b[1],b[2])}function n(a){var b=0,d,f,k,n,h,p,q=[];d=0;for(f=a.length;d<f;d++){h="[";p=['"'+a[d][0]+'"'];k=1;for(n=a[d].length;k<n;k++)p[k]="val["+b++ +"]";h+=p+"]";q[d]=h}return Function("val","return Snap.path.toString.call(["+q+"])")}function u(a){for(var b=[],d=0,f=a.length;d<f;d++)for(var k=1,n=a[d].length;k<n;k++)b.push(a[d][k]);return b}var p={},b=/[a-z]+$/i,q=String;
p.stroke=p.fill="colour";v.prototype.equal=function(a,b){return k("snap.util.equal",this,a,b).firstDefined()};k.on("snap.util.equal",function(e,k){var r,s;r=q(this.attr(e)||"");var x=this;if(r==+r&&k==+k)return{from:+r,to:+k,f:z};if("colour"==p[e])return r=a.color(r),s=a.color(k),{from:[r.r,r.g,r.b,r.opacity],to:[s.r,s.g,s.b,s.opacity],f:f};if("transform"==e||"gradientTransform"==e||"patternTransform"==e)return k instanceof a.Matrix&&(k=k.toTransformString()),a._.rgTransform.test(k)||(k=a._.svgTransform2string(k)),
w(r,k,function(){return x.getBBox(1)});if("d"==e||"path"==e)return r=a.path.toCubic(r,k),{from:u(r[0]),to:u(r[1]),f:n(r[0])};if("points"==e)return r=q(r).split(a._.separator),s=q(k).split(a._.separator),{from:r,to:s,f:function(a){return a}};aUnit=r.match(b);s=q(k).match(b);return aUnit&&aUnit==s?{from:parseFloat(r),to:parseFloat(k),f:d(aUnit)}:{from:this.asPX(e),to:this.asPX(e,k),f:z}})});C.plugin(function(a,v,y,C){var A=v.prototype,w="createTouch"in C.doc;v="click dblclick mousedown mousemove mouseout mouseover mouseup touchstart touchmove touchend touchcancel".split(" ");
var z={mousedown:"touchstart",mousemove:"touchmove",mouseup:"touchend"},d=function(a,b){var d="y"==a?"scrollTop":"scrollLeft",e=b&&b.node?b.node.ownerDocument:C.doc;return e[d in e.documentElement?"documentElement":"body"][d]},f=function(){this.returnValue=!1},n=function(){return this.originalEvent.preventDefault()},u=function(){this.cancelBubble=!0},p=function(){return this.originalEvent.stopPropagation()},b=function(){if(C.doc.addEventListener)return function(a,b,e,f){var k=w&&z[b]?z[b]:b,l=function(k){var l=
d("y",f),q=d("x",f);if(w&&z.hasOwnProperty(b))for(var r=0,u=k.targetTouches&&k.targetTouches.length;r<u;r++)if(k.targetTouches[r].target==a||a.contains(k.targetTouches[r].target)){u=k;k=k.targetTouches[r];k.originalEvent=u;k.preventDefault=n;k.stopPropagation=p;break}return e.call(f,k,k.clientX+q,k.clientY+l)};b!==k&&a.addEventListener(b,l,!1);a.addEventListener(k,l,!1);return function(){b!==k&&a.removeEventListener(b,l,!1);a.removeEventListener(k,l,!1);return!0}};if(C.doc.attachEvent)return function(a,
b,e,h){var k=function(a){a=a||h.node.ownerDocument.window.event;var b=d("y",h),k=d("x",h),k=a.clientX+k,b=a.clientY+b;a.preventDefault=a.preventDefault||f;a.stopPropagation=a.stopPropagation||u;return e.call(h,a,k,b)};a.attachEvent("on"+b,k);return function(){a.detachEvent("on"+b,k);return!0}}}(),q=[],e=function(a){for(var b=a.clientX,e=a.clientY,f=d("y"),l=d("x"),n,p=q.length;p--;){n=q[p];if(w)for(var r=a.touches&&a.touches.length,u;r--;){if(u=a.touches[r],u.identifier==n.el._drag.id||n.el.node.contains(u.target)){b=
u.clientX;e=u.clientY;(a.originalEvent?a.originalEvent:a).preventDefault();break}}else a.preventDefault();b+=l;e+=f;k("snap.drag.move."+n.el.id,n.move_scope||n.el,b-n.el._drag.x,e-n.el._drag.y,b,e,a)}},l=function(b){a.unmousemove(e).unmouseup(l);for(var d=q.length,f;d--;)f=q[d],f.el._drag={},k("snap.drag.end."+f.el.id,f.end_scope||f.start_scope||f.move_scope||f.el,b);q=[]};for(y=v.length;y--;)(function(d){a[d]=A[d]=function(e,f){a.is(e,"function")&&(this.events=this.events||[],this.events.push({name:d,
f:e,unbind:b(this.node||document,d,e,f||this)}));return this};a["un"+d]=A["un"+d]=function(a){for(var b=this.events||[],e=b.length;e--;)if(b[e].name==d&&(b[e].f==a||!a)){b[e].unbind();b.splice(e,1);!b.length&&delete this.events;break}return this}})(v[y]);A.hover=function(a,b,d,e){return this.mouseover(a,d).mouseout(b,e||d)};A.unhover=function(a,b){return this.unmouseover(a).unmouseout(b)};var r=[];A.drag=function(b,d,f,h,n,p){function u(r,v,w){(r.originalEvent||r).preventDefault();this._drag.x=v;
this._drag.y=w;this._drag.id=r.identifier;!q.length&&a.mousemove(e).mouseup(l);q.push({el:this,move_scope:h,start_scope:n,end_scope:p});d&&k.on("snap.drag.start."+this.id,d);b&&k.on("snap.drag.move."+this.id,b);f&&k.on("snap.drag.end."+this.id,f);k("snap.drag.start."+this.id,n||h||this,v,w,r)}if(!arguments.length){var v;return this.drag(function(a,b){this.attr({transform:v+(v?"T":"t")+[a,b]})},function(){v=this.transform().local})}this._drag={};r.push({el:this,start:u});this.mousedown(u);return this};
A.undrag=function(){for(var b=r.length;b--;)r[b].el==this&&(this.unmousedown(r[b].start),r.splice(b,1),k.unbind("snap.drag.*."+this.id));!r.length&&a.unmousemove(e).unmouseup(l);return this}});C.plugin(function(a,v,y,C){y=y.prototype;var A=/^\s*url\((.+)\)/,w=String,z=a._.$;a.filter={};y.filter=function(d){var f=this;"svg"!=f.type&&(f=f.paper);d=a.parse(w(d));var k=a._.id(),u=z("filter");z(u,{id:k,filterUnits:"userSpaceOnUse"});u.appendChild(d.node);f.defs.appendChild(u);return new v(u)};k.on("snap.util.getattr.filter",
function(){k.stop();var d=z(this.node,"filter");if(d)return(d=w(d).match(A))&&a.select(d[1])});k.on("snap.util.attr.filter",function(d){if(d instanceof v&&"filter"==d.type){k.stop();var f=d.node.id;f||(z(d.node,{id:d.id}),f=d.id);z(this.node,{filter:a.url(f)})}d&&"none"!=d||(k.stop(),this.node.removeAttribute("filter"))});a.filter.blur=function(d,f){null==d&&(d=2);return a.format('<feGaussianBlur stdDeviation="{def}"/>',{def:null==f?d:[d,f]})};a.filter.blur.toString=function(){return this()};a.filter.shadow=
function(d,f,k,u,p){"string"==typeof k&&(p=u=k,k=4);"string"!=typeof u&&(p=u,u="#000");null==k&&(k=4);null==p&&(p=1);null==d&&(d=0,f=2);null==f&&(f=d);u=a.color(u||"#000");return a.format('<feGaussianBlur in="SourceAlpha" stdDeviation="{blur}"/><feOffset dx="{dx}" dy="{dy}" result="offsetblur"/><feFlood flood-color="{color}"/><feComposite in2="offsetblur" operator="in"/><feComponentTransfer><feFuncA type="linear" slope="{opacity}"/></feComponentTransfer><feMerge><feMergeNode/><feMergeNode in="SourceGraphic"/></feMerge>',
{color:u,dx:d,dy:f,blur:k,opacity:p})};a.filter.shadow.toString=function(){return this()};a.filter.grayscale=function(d){null==d&&(d=1);return a.format('<feColorMatrix type="matrix" values="{a} {b} {c} 0 0 {d} {e} {f} 0 0 {g} {b} {h} 0 0 0 0 0 1 0"/>',{a:0.2126+0.7874*(1-d),b:0.7152-0.7152*(1-d),c:0.0722-0.0722*(1-d),d:0.2126-0.2126*(1-d),e:0.7152+0.2848*(1-d),f:0.0722-0.0722*(1-d),g:0.2126-0.2126*(1-d),h:0.0722+0.9278*(1-d)})};a.filter.grayscale.toString=function(){return this()};a.filter.sepia=
function(d){null==d&&(d=1);return a.format('<feColorMatrix type="matrix" values="{a} {b} {c} 0 0 {d} {e} {f} 0 0 {g} {h} {i} 0 0 0 0 0 1 0"/>',{a:0.393+0.607*(1-d),b:0.769-0.769*(1-d),c:0.189-0.189*(1-d),d:0.349-0.349*(1-d),e:0.686+0.314*(1-d),f:0.168-0.168*(1-d),g:0.272-0.272*(1-d),h:0.534-0.534*(1-d),i:0.131+0.869*(1-d)})};a.filter.sepia.toString=function(){return this()};a.filter.saturate=function(d){null==d&&(d=1);return a.format('<feColorMatrix type="saturate" values="{amount}"/>',{amount:1-
d})};a.filter.saturate.toString=function(){return this()};a.filter.hueRotate=function(d){return a.format('<feColorMatrix type="hueRotate" values="{angle}"/>',{angle:d||0})};a.filter.hueRotate.toString=function(){return this()};a.filter.invert=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="table" tableValues="{amount} {amount2}"/><feFuncG type="table" tableValues="{amount} {amount2}"/><feFuncB type="table" tableValues="{amount} {amount2}"/></feComponentTransfer>',{amount:d,
amount2:1-d})};a.filter.invert.toString=function(){return this()};a.filter.brightness=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="linear" slope="{amount}"/><feFuncG type="linear" slope="{amount}"/><feFuncB type="linear" slope="{amount}"/></feComponentTransfer>',{amount:d})};a.filter.brightness.toString=function(){return this()};a.filter.contrast=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="linear" slope="{amount}" intercept="{amount2}"/><feFuncG type="linear" slope="{amount}" intercept="{amount2}"/><feFuncB type="linear" slope="{amount}" intercept="{amount2}"/></feComponentTransfer>',
{amount:d,amount2:0.5-d/2})};a.filter.contrast.toString=function(){return this()}});return C});

]]> </script>
<script> <![CDATA[

(function (glob, factory) {
    // AMD support
    if (typeof define === "function" && define.amd) {
        // Define as an anonymous module
        define("Gadfly", ["Snap.svg"], function (Snap) {
            return factory(Snap);
        });
    } else {
        // Browser globals (glob is window)
        // Snap adds itself to window
        glob.Gadfly = factory(glob.Snap);
    }
}(this, function (Snap) {

var Gadfly = {};

// Get an x/y coordinate value in pixels
var xPX = function(fig, x) {
    var client_box = fig.node.getBoundingClientRect();
    return x * fig.node.viewBox.baseVal.width / client_box.width;
};

var yPX = function(fig, y) {
    var client_box = fig.node.getBoundingClientRect();
    return y * fig.node.viewBox.baseVal.height / client_box.height;
};


Snap.plugin(function (Snap, Element, Paper, global) {
    // Traverse upwards from a snap element to find and return the first
    // note with the "plotroot" class.
    Element.prototype.plotroot = function () {
        var element = this;
        while (!element.hasClass("plotroot") && element.parent() != null) {
            element = element.parent();
        }
        return element;
    };

    Element.prototype.svgroot = function () {
        var element = this;
        while (element.node.nodeName != "svg" && element.parent() != null) {
            element = element.parent();
        }
        return element;
    };

    Element.prototype.plotbounds = function () {
        var root = this.plotroot()
        var bbox = root.select(".guide.background").node.getBBox();
        return {
            x0: bbox.x,
            x1: bbox.x + bbox.width,
            y0: bbox.y,
            y1: bbox.y + bbox.height
        };
    };

    Element.prototype.plotcenter = function () {
        var root = this.plotroot()
        var bbox = root.select(".guide.background").node.getBBox();
        return {
            x: bbox.x + bbox.width / 2,
            y: bbox.y + bbox.height / 2
        };
    };

    // Emulate IE style mouseenter/mouseleave events, since Microsoft always
    // does everything right.
    // See: http://www.dynamic-tools.net/toolbox/isMouseLeaveOrEnter/
    var events = ["mouseenter", "mouseleave"];

    for (i in events) {
        (function (event_name) {
            var event_name = events[i];
            Element.prototype[event_name] = function (fn, scope) {
                if (Snap.is(fn, "function")) {
                    var fn2 = function (event) {
                        if (event.type != "mouseover" && event.type != "mouseout") {
                            return;
                        }

                        var reltg = event.relatedTarget ? event.relatedTarget :
                            event.type == "mouseout" ? event.toElement : event.fromElement;
                        while (reltg && reltg != this.node) reltg = reltg.parentNode;

                        if (reltg != this.node) {
                            return fn.apply(this, event);
                        }
                    };

                    if (event_name == "mouseenter") {
                        this.mouseover(fn2, scope);
                    } else {
                        this.mouseout(fn2, scope);
                    }
                }
                return this;
            };
        })(events[i]);
    }


    Element.prototype.mousewheel = function (fn, scope) {
        if (Snap.is(fn, "function")) {
            var el = this;
            var fn2 = function (event) {
                fn.apply(el, [event]);
            };
        }

        this.node.addEventListener(
            /Firefox/i.test(navigator.userAgent) ? "DOMMouseScroll" : "mousewheel",
            fn2);

        return this;
    };


    // Snap's attr function can be too slow for things like panning/zooming.
    // This is a function to directly update element attributes without going
    // through eve.
    Element.prototype.attribute = function(key, val) {
        if (val === undefined) {
            return this.node.getAttribute(key);
        } else {
            this.node.setAttribute(key, val);
            return this;
        }
    };

    Element.prototype.init_gadfly = function() {
        this.mouseenter(Gadfly.plot_mouseover)
            .mouseleave(Gadfly.plot_mouseout)
            .dblclick(Gadfly.plot_dblclick)
            .mousewheel(Gadfly.guide_background_scroll)
            .drag(Gadfly.guide_background_drag_onmove,
                  Gadfly.guide_background_drag_onstart,
                  Gadfly.guide_background_drag_onend);
        this.mouseenter(function (event) {
            init_pan_zoom(this.plotroot());
        });
        return this;
    };
});


// When the plot is moused over, emphasize the grid lines.
Gadfly.plot_mouseover = function(event) {
    var root = this.plotroot();

    var keyboard_zoom = function(event) {
        if (event.which == 187) { // plus
            increase_zoom_by_position(root, 0.1, true);
        } else if (event.which == 189) { // minus
            increase_zoom_by_position(root, -0.1, true);
        }
    };
    root.data("keyboard_zoom", keyboard_zoom);
    window.addEventListener("keyup", keyboard_zoom);

    var xgridlines = root.select(".xgridlines"),
        ygridlines = root.select(".ygridlines");

    xgridlines.data("unfocused_strokedash",
                    xgridlines.attribute("stroke-dasharray").replace(/(\d)(,|$)/g, "$1mm$2"));
    ygridlines.data("unfocused_strokedash",
                    ygridlines.attribute("stroke-dasharray").replace(/(\d)(,|$)/g, "$1mm$2"));

    // emphasize grid lines
    var destcolor = root.data("focused_xgrid_color");
    xgridlines.attribute("stroke-dasharray", "none")
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    destcolor = root.data("focused_ygrid_color");
    ygridlines.attribute("stroke-dasharray", "none")
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    // reveal zoom slider
    root.select(".zoomslider")
        .animate({opacity: 1.0}, 250);
};

// Reset pan and zoom on double click
Gadfly.plot_dblclick = function(event) {
  set_plot_pan_zoom(this.plotroot(), 0.0, 0.0, 1.0);
};

// Unemphasize grid lines on mouse out.
Gadfly.plot_mouseout = function(event) {
    var root = this.plotroot();

    window.removeEventListener("keyup", root.data("keyboard_zoom"));
    root.data("keyboard_zoom", undefined);

    var xgridlines = root.select(".xgridlines"),
        ygridlines = root.select(".ygridlines");

    var destcolor = root.data("unfocused_xgrid_color");

    xgridlines.attribute("stroke-dasharray", xgridlines.data("unfocused_strokedash"))
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    destcolor = root.data("unfocused_ygrid_color");
    ygridlines.attribute("stroke-dasharray", ygridlines.data("unfocused_strokedash"))
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    // hide zoom slider
    root.select(".zoomslider")
        .animate({opacity: 0.0}, 250);
};


var set_geometry_transform = function(root, tx, ty, scale) {
    var xscalable = root.hasClass("xscalable"),
        yscalable = root.hasClass("yscalable");

    var old_scale = root.data("scale");

    var xscale = xscalable ? scale : 1.0,
        yscale = yscalable ? scale : 1.0;

    tx = xscalable ? tx : 0.0;
    ty = yscalable ? ty : 0.0;

    var t = new Snap.Matrix().translate(tx, ty).scale(xscale, yscale);

    root.selectAll(".geometry, image")
        .forEach(function (element, i) {
            element.transform(t);
        });

    bounds = root.plotbounds();

    if (yscalable) {
        var xfixed_t = new Snap.Matrix().translate(0, ty).scale(1.0, yscale);
        root.selectAll(".xfixed")
            .forEach(function (element, i) {
                element.transform(xfixed_t);
            });

        root.select(".ylabels")
            .transform(xfixed_t)
            .selectAll("text")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var cx = element.asPX("x"),
                        cy = element.asPX("y");
                    var st = element.data("static_transform");
                    unscale_t = new Snap.Matrix();
                    unscale_t.scale(1, 1/scale, cx, cy).add(st);
                    element.transform(unscale_t);

                    var y = cy * scale + ty;
                    element.attr("visibility",
                        bounds.y0 <= y && y <= bounds.y1 ? "visible" : "hidden");
                }
            });
    }

    if (xscalable) {
        var yfixed_t = new Snap.Matrix().translate(tx, 0).scale(xscale, 1.0);
        var xtrans = new Snap.Matrix().translate(tx, 0);
        root.selectAll(".yfixed")
            .forEach(function (element, i) {
                element.transform(yfixed_t);
            });

        root.select(".xlabels")
            .transform(yfixed_t)
            .selectAll("text")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var cx = element.asPX("x"),
                        cy = element.asPX("y");
                    var st = element.data("static_transform");
                    unscale_t = new Snap.Matrix();
                    unscale_t.scale(1/scale, 1, cx, cy).add(st);

                    element.transform(unscale_t);

                    var x = cx * scale + tx;
                    element.attr("visibility",
                        bounds.x0 <= x && x <= bounds.x1 ? "visible" : "hidden");
                    }
            });
    }

    // we must unscale anything that is scale invariance: widths, raiduses, etc.
    var size_attribs = ["font-size"];
    var unscaled_selection = ".geometry, .geometry *";
    if (xscalable) {
        size_attribs.push("rx");
        unscaled_selection += ", .xgridlines";
    }
    if (yscalable) {
        size_attribs.push("ry");
        unscaled_selection += ", .ygridlines";
    }

    root.selectAll(unscaled_selection)
        .forEach(function (element, i) {
            // circle need special help
            if (element.node.nodeName == "circle") {
                var cx = element.attribute("cx"),
                    cy = element.attribute("cy");
                unscale_t = new Snap.Matrix().scale(1/xscale, 1/yscale,
                                                        cx, cy);
                element.transform(unscale_t);
                return;
            }

            for (i in size_attribs) {
                var key = size_attribs[i];
                var val = parseFloat(element.attribute(key));
                if (val !== undefined && val != 0 && !isNaN(val)) {
                    element.attribute(key, val * old_scale / scale);
                }
            }
        });
};


// Find the most appropriate tick scale and update label visibility.
var update_tickscale = function(root, scale, axis) {
    if (!root.hasClass(axis + "scalable")) return;

    var tickscales = root.data(axis + "tickscales");
    var best_tickscale = 1.0;
    var best_tickscale_dist = Infinity;
    for (tickscale in tickscales) {
        var dist = Math.abs(Math.log(tickscale) - Math.log(scale));
        if (dist < best_tickscale_dist) {
            best_tickscale_dist = dist;
            best_tickscale = tickscale;
        }
    }

    if (best_tickscale != root.data(axis + "tickscale")) {
        root.data(axis + "tickscale", best_tickscale);
        var mark_inscale_gridlines = function (element, i) {
            var inscale = element.attr("gadfly:scale") == best_tickscale;
            element.attribute("gadfly:inscale", inscale);
            element.attr("visibility", inscale ? "visible" : "hidden");
        };

        var mark_inscale_labels = function (element, i) {
            var inscale = element.attr("gadfly:scale") == best_tickscale;
            element.attribute("gadfly:inscale", inscale);
            element.attr("visibility", inscale ? "visible" : "hidden");
        };

        root.select("." + axis + "gridlines").selectAll("path").forEach(mark_inscale_gridlines);
        root.select("." + axis + "labels").selectAll("text").forEach(mark_inscale_labels);
    }
};


var set_plot_pan_zoom = function(root, tx, ty, scale) {
    var old_scale = root.data("scale");
    var bounds = root.plotbounds();

    var width = bounds.x1 - bounds.x0,
        height = bounds.y1 - bounds.y0;

    // compute the viewport derived from tx, ty, and scale
    var x_min = -width * scale - (scale * width - width),
        x_max = width * scale,
        y_min = -height * scale - (scale * height - height),
        y_max = height * scale;

    var x0 = bounds.x0 - scale * bounds.x0,
        y0 = bounds.y0 - scale * bounds.y0;

    var tx = Math.max(Math.min(tx - x0, x_max), x_min),
        ty = Math.max(Math.min(ty - y0, y_max), y_min);

    tx += x0;
    ty += y0;

    // when the scale change, we may need to alter which set of
    // ticks is being displayed
    if (scale != old_scale) {
        update_tickscale(root, scale, "x");
        update_tickscale(root, scale, "y");
    }

    set_geometry_transform(root, tx, ty, scale);

    root.data("scale", scale);
    root.data("tx", tx);
    root.data("ty", ty);
};


var scale_centered_translation = function(root, scale) {
    var bounds = root.plotbounds();

    var width = bounds.x1 - bounds.x0,
        height = bounds.y1 - bounds.y0;

    var tx0 = root.data("tx"),
        ty0 = root.data("ty");

    var scale0 = root.data("scale");

    // how off from center the current view is
    var xoff = tx0 - (bounds.x0 * (1 - scale0) + (width * (1 - scale0)) / 2),
        yoff = ty0 - (bounds.y0 * (1 - scale0) + (height * (1 - scale0)) / 2);

    // rescale offsets
    xoff = xoff * scale / scale0;
    yoff = yoff * scale / scale0;

    // adjust for the panel position being scaled
    var x_edge_adjust = bounds.x0 * (1 - scale),
        y_edge_adjust = bounds.y0 * (1 - scale);

    return {
        x: xoff + x_edge_adjust + (width - width * scale) / 2,
        y: yoff + y_edge_adjust + (height - height * scale) / 2
    };
};


// Initialize data for panning zooming if it isn't already.
var init_pan_zoom = function(root) {
    if (root.data("zoompan-ready")) {
        return;
    }

    // The non-scaling-stroke trick. Rather than try to correct for the
    // stroke-width when zooming, we force it to a fixed value.
    var px_per_mm = root.node.getCTM().a;

    // Drag events report deltas in pixels, which we'd like to convert to
    // millimeters.
    root.data("px_per_mm", px_per_mm);

    root.selectAll("path")
        .forEach(function (element, i) {
        sw = element.asPX("stroke-width") * px_per_mm;
        if (sw > 0) {
            element.attribute("stroke-width", sw);
            element.attribute("vector-effect", "non-scaling-stroke");
        }
    });

    // Store ticks labels original tranformation
    root.selectAll(".xlabels > text, .ylabels > text")
        .forEach(function (element, i) {
            var lm = element.transform().localMatrix;
            element.data("static_transform",
                new Snap.Matrix(lm.a, lm.b, lm.c, lm.d, lm.e, lm.f));
        });

    var xgridlines = root.select(".xgridlines");
    var ygridlines = root.select(".ygridlines");
    var xlabels = root.select(".xlabels");
    var ylabels = root.select(".ylabels");

    if (root.data("tx") === undefined) root.data("tx", 0);
    if (root.data("ty") === undefined) root.data("ty", 0);
    if (root.data("scale") === undefined) root.data("scale", 1.0);
    if (root.data("xtickscales") === undefined) {

        // index all the tick scales that are listed
        var xtickscales = {};
        var ytickscales = {};
        var add_x_tick_scales = function (element, i) {
            xtickscales[element.attribute("gadfly:scale")] = true;
        };
        var add_y_tick_scales = function (element, i) {
            ytickscales[element.attribute("gadfly:scale")] = true;
        };

        if (xgridlines) xgridlines.selectAll("path").forEach(add_x_tick_scales);
        if (ygridlines) ygridlines.selectAll("path").forEach(add_y_tick_scales);
        if (xlabels) xlabels.selectAll("text").forEach(add_x_tick_scales);
        if (ylabels) ylabels.selectAll("text").forEach(add_y_tick_scales);

        root.data("xtickscales", xtickscales);
        root.data("ytickscales", ytickscales);
        root.data("xtickscale", 1.0);
    }

    var min_scale = 1.0, max_scale = 1.0;
    for (scale in xtickscales) {
        min_scale = Math.min(min_scale, scale);
        max_scale = Math.max(max_scale, scale);
    }
    for (scale in ytickscales) {
        min_scale = Math.min(min_scale, scale);
        max_scale = Math.max(max_scale, scale);
    }
    root.data("min_scale", min_scale);
    root.data("max_scale", max_scale);

    // store the original positions of labels
    if (xlabels) {
        xlabels.selectAll("text")
               .forEach(function (element, i) {
                   element.data("x", element.asPX("x"));
               });
    }

    if (ylabels) {
        ylabels.selectAll("text")
               .forEach(function (element, i) {
                   element.data("y", element.asPX("y"));
               });
    }

    // mark grid lines and ticks as in or out of scale.
    var mark_inscale = function (element, i) {
        element.attribute("gadfly:inscale", element.attribute("gadfly:scale") == 1.0);
    };

    if (xgridlines) xgridlines.selectAll("path").forEach(mark_inscale);
    if (ygridlines) ygridlines.selectAll("path").forEach(mark_inscale);
    if (xlabels) xlabels.selectAll("text").forEach(mark_inscale);
    if (ylabels) ylabels.selectAll("text").forEach(mark_inscale);

    // figure out the upper ond lower bounds on panning using the maximum
    // and minum grid lines
    var bounds = root.plotbounds();
    var pan_bounds = {
        x0: 0.0,
        y0: 0.0,
        x1: 0.0,
        y1: 0.0
    };

    if (xgridlines) {
        xgridlines
            .selectAll("path")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var bbox = element.node.getBBox();
                    if (bounds.x1 - bbox.x < pan_bounds.x0) {
                        pan_bounds.x0 = bounds.x1 - bbox.x;
                    }
                    if (bounds.x0 - bbox.x > pan_bounds.x1) {
                        pan_bounds.x1 = bounds.x0 - bbox.x;
                    }
                    element.attr("visibility", "visible");
                }
            });
    }

    if (ygridlines) {
        ygridlines
            .selectAll("path")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var bbox = element.node.getBBox();
                    if (bounds.y1 - bbox.y < pan_bounds.y0) {
                        pan_bounds.y0 = bounds.y1 - bbox.y;
                    }
                    if (bounds.y0 - bbox.y > pan_bounds.y1) {
                        pan_bounds.y1 = bounds.y0 - bbox.y;
                    }
                    element.attr("visibility", "visible");
                }
            });
    }

    // nudge these values a little
    pan_bounds.x0 -= 5;
    pan_bounds.x1 += 5;
    pan_bounds.y0 -= 5;
    pan_bounds.y1 += 5;
    root.data("pan_bounds", pan_bounds);

    root.data("zoompan-ready", true)
};


// drag actions, i.e. zooming and panning
var pan_action = {
    start: function(root, x, y, event) {
        root.data("dx", 0);
        root.data("dy", 0);
        root.data("tx0", root.data("tx"));
        root.data("ty0", root.data("ty"));
    },
    update: function(root, dx, dy, x, y, event) {
        var px_per_mm = root.data("px_per_mm");
        dx /= px_per_mm;
        dy /= px_per_mm;

        var tx0 = root.data("tx"),
            ty0 = root.data("ty");

        var dx0 = root.data("dx"),
            dy0 = root.data("dy");

        root.data("dx", dx);
        root.data("dy", dy);

        dx = dx - dx0;
        dy = dy - dy0;

        var tx = tx0 + dx,
            ty = ty0 + dy;

        set_plot_pan_zoom(root, tx, ty, root.data("scale"));
    },
    end: function(root, event) {

    },
    cancel: function(root) {
        set_plot_pan_zoom(root, root.data("tx0"), root.data("ty0"), root.data("scale"));
    }
};

var zoom_box;
var zoom_action = {
    start: function(root, x, y, event) {
        var bounds = root.plotbounds();
        var width = bounds.x1 - bounds.x0,
            height = bounds.y1 - bounds.y0;
        var ratio = width / height;
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var px_per_mm = root.data("px_per_mm");
        x = xscalable ? x / px_per_mm : bounds.x0;
        y = yscalable ? y / px_per_mm : bounds.y0;
        var w = xscalable ? 0 : width;
        var h = yscalable ? 0 : height;
        zoom_box = root.rect(x, y, w, h).attr({
            "fill": "#000",
            "opacity": 0.25
        });
        zoom_box.data("ratio", ratio);
    },
    update: function(root, dx, dy, x, y, event) {
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var px_per_mm = root.data("px_per_mm");
        var bounds = root.plotbounds();
        if (yscalable) {
            y /= px_per_mm;
            y = Math.max(bounds.y0, y);
            y = Math.min(bounds.y1, y);
        } else {
            y = bounds.y1;
        }
        if (xscalable) {
            x /= px_per_mm;
            x = Math.max(bounds.x0, x);
            x = Math.min(bounds.x1, x);
        } else {
            x = bounds.x1;
        }

        dx = x - zoom_box.attr("x");
        dy = y - zoom_box.attr("y");
        if (xscalable && yscalable) {
            var ratio = zoom_box.data("ratio");
            var width = Math.min(Math.abs(dx), ratio * Math.abs(dy));
            var height = Math.min(Math.abs(dy), Math.abs(dx) / ratio);
            dx = width * dx / Math.abs(dx);
            dy = height * dy / Math.abs(dy);
        }
        var xoffset = 0,
            yoffset = 0;
        if (dx < 0) {
            xoffset = dx;
            dx = -1 * dx;
        }
        if (dy < 0) {
            yoffset = dy;
            dy = -1 * dy;
        }
        if (isNaN(dy)) {
            dy = 0.0;
        }
        if (isNaN(dx)) {
            dx = 0.0;
        }
        zoom_box.transform("T" + xoffset + "," + yoffset);
        zoom_box.attr("width", dx);
        zoom_box.attr("height", dy);
    },
    end: function(root, event) {
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var zoom_bounds = zoom_box.getBBox();
        if (zoom_bounds.width * zoom_bounds.height <= 0) {
            return;
        }
        var plot_bounds = root.plotbounds();
        var zoom_factor = 1.0;
        if (yscalable) {
            zoom_factor = (plot_bounds.y1 - plot_bounds.y0) / zoom_bounds.height;
        } else {
            zoom_factor = (plot_bounds.x1 - plot_bounds.x0) / zoom_bounds.width;
        }
        var tx = (root.data("tx") - zoom_bounds.x) * zoom_factor + plot_bounds.x0,
            ty = (root.data("ty") - zoom_bounds.y) * zoom_factor + plot_bounds.y0;
        set_plot_pan_zoom(root, tx, ty, root.data("scale") * zoom_factor);
        zoom_box.remove();
    },
    cancel: function(root) {
        zoom_box.remove();
    }
};


Gadfly.guide_background_drag_onstart = function(x, y, event) {
    var root = this.plotroot();
    var scalable = root.hasClass("xscalable") || root.hasClass("yscalable");
    var zoomable = !event.altKey && !event.ctrlKey && event.shiftKey && scalable;
    var panable = !event.altKey && !event.ctrlKey && !event.shiftKey && scalable;
    var drag_action = zoomable ? zoom_action :
                      panable  ? pan_action :
                                 undefined;
    root.data("drag_action", drag_action);
    if (drag_action) {
        var cancel_drag_action = function(event) {
            if (event.which == 27) { // esc key
                drag_action.cancel(root);
                root.data("drag_action", undefined);
            }
        };
        window.addEventListener("keyup", cancel_drag_action);
        root.data("cancel_drag_action", cancel_drag_action);
        drag_action.start(root, x, y, event);
    }
};


Gadfly.guide_background_drag_onmove = function(dx, dy, x, y, event) {
    var root = this.plotroot();
    var drag_action = root.data("drag_action");
    if (drag_action) {
        drag_action.update(root, dx, dy, x, y, event);
    }
};


Gadfly.guide_background_drag_onend = function(event) {
    var root = this.plotroot();
    window.removeEventListener("keyup", root.data("cancel_drag_action"));
    root.data("cancel_drag_action", undefined);
    var drag_action = root.data("drag_action");
    if (drag_action) {
        drag_action.end(root, event);
    }
    root.data("drag_action", undefined);
};


Gadfly.guide_background_scroll = function(event) {
    if (event.shiftKey) {
        increase_zoom_by_position(this.plotroot(), 0.001 * event.wheelDelta);
        event.preventDefault();
    }
};


Gadfly.zoomslider_button_mouseover = function(event) {
    this.select(".button_logo")
         .animate({fill: this.data("mouseover_color")}, 100);
};


Gadfly.zoomslider_button_mouseout = function(event) {
     this.select(".button_logo")
         .animate({fill: this.data("mouseout_color")}, 100);
};


Gadfly.zoomslider_zoomout_click = function(event) {
    increase_zoom_by_position(this.plotroot(), -0.1, true);
};


Gadfly.zoomslider_zoomin_click = function(event) {
    increase_zoom_by_position(this.plotroot(), 0.1, true);
};


Gadfly.zoomslider_track_click = function(event) {
    // TODO
};


// Map slider position x to scale y using the function y = a*exp(b*x)+c.
// The constants a, b, and c are solved using the constraint that the function
// should go through the points (0; min_scale), (0.5; 1), and (1; max_scale).
var scale_from_slider_position = function(position, min_scale, max_scale) {
    var a = (1 - 2 * min_scale + min_scale * min_scale) / (min_scale + max_scale - 2),
        b = 2 * Math.log((max_scale - 1) / (1 - min_scale)),
        c = (min_scale * max_scale - 1) / (min_scale + max_scale - 2);
    return a * Math.exp(b * position) + c;
}

// inverse of scale_from_slider_position
var slider_position_from_scale = function(scale, min_scale, max_scale) {
    var a = (1 - 2 * min_scale + min_scale * min_scale) / (min_scale + max_scale - 2),
        b = 2 * Math.log((max_scale - 1) / (1 - min_scale)),
        c = (min_scale * max_scale - 1) / (min_scale + max_scale - 2);
    return 1 / b * Math.log((scale - c) / a);
}

var increase_zoom_by_position = function(root, delta_position, animate) {
    var scale = root.data("scale"),
        min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale");
    var position = slider_position_from_scale(scale, min_scale, max_scale);
    position += delta_position;
    scale = scale_from_slider_position(position, min_scale, max_scale);
    set_zoom(root, scale, animate);
}

var set_zoom = function(root, scale, animate) {
    var min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale"),
        old_scale = root.data("scale");
    var new_scale = Math.max(min_scale, Math.min(scale, max_scale));
    if (animate) {
        Snap.animate(
            old_scale,
            new_scale,
            function (new_scale) {
                update_plot_scale(root, new_scale);
            },
            200);
    } else {
        update_plot_scale(root, new_scale);
    }
}


var update_plot_scale = function(root, new_scale) {
    var trans = scale_centered_translation(root, new_scale);
    set_plot_pan_zoom(root, trans.x, trans.y, new_scale);

    root.selectAll(".zoomslider_thumb")
        .forEach(function (element, i) {
            var min_pos = element.data("min_pos"),
                max_pos = element.data("max_pos"),
                min_scale = root.data("min_scale"),
                max_scale = root.data("max_scale");
            var xmid = (min_pos + max_pos) / 2;
            var xpos = slider_position_from_scale(new_scale, min_scale, max_scale);
            element.transform(new Snap.Matrix().translate(
                Math.max(min_pos, Math.min(
                         max_pos, min_pos + (max_pos - min_pos) * xpos)) - xmid, 0));
    });
};


Gadfly.zoomslider_thumb_dragmove = function(dx, dy, x, y, event) {
    var root = this.plotroot();
    var min_pos = this.data("min_pos"),
        max_pos = this.data("max_pos"),
        min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale"),
        old_scale = root.data("old_scale");

    var px_per_mm = root.data("px_per_mm");
    dx /= px_per_mm;
    dy /= px_per_mm;

    var xmid = (min_pos + max_pos) / 2;
    var xpos = slider_position_from_scale(old_scale, min_scale, max_scale) +
                   dx / (max_pos - min_pos);

    // compute the new scale
    var new_scale = scale_from_slider_position(xpos, min_scale, max_scale);
    new_scale = Math.min(max_scale, Math.max(min_scale, new_scale));

    update_plot_scale(root, new_scale);
    event.stopPropagation();
};


Gadfly.zoomslider_thumb_dragstart = function(x, y, event) {
    this.animate({fill: this.data("mouseover_color")}, 100);
    var root = this.plotroot();

    // keep track of what the scale was when we started dragging
    root.data("old_scale", root.data("scale"));
    event.stopPropagation();
};


Gadfly.zoomslider_thumb_dragend = function(event) {
    this.animate({fill: this.data("mouseout_color")}, 100);
    event.stopPropagation();
};


var toggle_color_class = function(root, color_class, ison) {
    var guides = root.selectAll(".guide." + color_class + ",.guide ." + color_class);
    var geoms = root.selectAll(".geometry." + color_class + ",.geometry ." + color_class);
    if (ison) {
        guides.animate({opacity: 0.5}, 250);
        geoms.animate({opacity: 0.0}, 250);
    } else {
        guides.animate({opacity: 1.0}, 250);
        geoms.animate({opacity: 1.0}, 250);
    }
};


Gadfly.colorkey_swatch_click = function(event) {
    var root = this.plotroot();
    var color_class = this.data("color_class");

    if (event.shiftKey) {
        root.selectAll(".colorkey text")
            .forEach(function (element) {
                var other_color_class = element.data("color_class");
                if (other_color_class != color_class) {
                    toggle_color_class(root, other_color_class,
                                       element.attr("opacity") == 1.0);
                }
            });
    } else {
        toggle_color_class(root, color_class, this.attr("opacity") == 1.0);
    }
};


return Gadfly;

}));


//@ sourceURL=gadfly.js

(function (glob, factory) {
    // AMD support
      if (typeof require === "function" && typeof define === "function" && define.amd) {
        require(["Snap.svg", "Gadfly"], function (Snap, Gadfly) {
            factory(Snap, Gadfly);
        });
      } else {
          factory(glob.Snap, glob.Gadfly);
      }
})(window, function (Snap, Gadfly) {
    var fig = Snap("#img-2f634ffe");
fig.select("#img-2f634ffe-4")
   .drag(function() {}, function() {}, function() {});
fig.select("#img-2f634ffe-6")
   .data("color_class", "color_Emission")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-2f634ffe-7")
   .data("color_class", "color_SoilMoisture")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-2f634ffe-8")
   .data("color_class", "color_t2m")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-2f634ffe-10")
   .data("color_class", "color_Emission")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-2f634ffe-11")
   .data("color_class", "color_SoilMoisture")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-2f634ffe-12")
   .data("color_class", "color_t2m")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-2f634ffe-16")
   .init_gadfly();
fig.select("#img-2f634ffe-19")
   .plotroot().data("unfocused_ygrid_color", "#D0D0E0")
;
fig.select("#img-2f634ffe-19")
   .plotroot().data("focused_ygrid_color", "#A0A0A0")
;
fig.select("#img-2f634ffe-114")
   .plotroot().data("unfocused_xgrid_color", "#D0D0E0")
;
fig.select("#img-2f634ffe-114")
   .plotroot().data("focused_xgrid_color", "#A0A0A0")
;
fig.select("#img-2f634ffe-153")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-2f634ffe-153")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-2f634ffe-153")
   .click(Gadfly.zoomslider_zoomin_click)
.mouseenter(Gadfly.zoomslider_button_mouseover)
.mouseleave(Gadfly.zoomslider_button_mouseout)
;
fig.select("#img-2f634ffe-157")
   .data("max_pos", 101.45)
;
fig.select("#img-2f634ffe-157")
   .data("min_pos", 84.45)
;
fig.select("#img-2f634ffe-157")
   .click(Gadfly.zoomslider_track_click);
fig.select("#img-2f634ffe-159")
   .data("max_pos", 101.45)
;
fig.select("#img-2f634ffe-159")
   .data("min_pos", 84.45)
;
fig.select("#img-2f634ffe-159")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-2f634ffe-159")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-2f634ffe-159")
   .drag(Gadfly.zoomslider_thumb_dragmove,
     Gadfly.zoomslider_thumb_dragstart,
     Gadfly.zoomslider_thumb_dragend)
;
fig.select("#img-2f634ffe-161")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-2f634ffe-161")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-2f634ffe-161")
   .click(Gadfly.zoomslider_zoomout_click)
.mouseenter(Gadfly.zoomslider_button_mouseover)
.mouseleave(Gadfly.zoomslider_button_mouseout)
;
    });
]]> </script>
</svg>




```julia
plotTS(cube_anomalies)
```






















<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     xmlns:gadfly="http://www.gadflyjl.org/ns"
     version="1.2"
     width="141.42mm" height="100mm" viewBox="0 0 141.42 100"
     stroke="none"
     fill="#000000"
     stroke-width="0.3"
     font-size="3.88"

     id="img-9020955a">
<g class="plotroot xscalable yscalable" id="img-9020955a-1">
  <g font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" fill="#564A55" stroke="#000000" stroke-opacity="0.000" id="img-9020955a-2">
    <text x="67.36" y="88.39" text-anchor="middle" dy="0.6em">x</text>
  </g>
  <g class="guide xlabels" font-size="2.82" font-family="'PT Sans Caption','Helvetica Neue','Helvetica',sans-serif" fill="#6C606B" id="img-9020955a-3">
    <text x="-108.98" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">Jan 1, 1980</text>
    <text x="-76.91" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1985</text>
    <text x="-44.85" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1990</text>
    <text x="-12.79" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1995</text>
    <text x="19.27" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2000</text>
    <text x="51.34" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2005</text>
    <text x="83.4" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2010</text>
    <text x="115.45" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2015</text>
    <text x="147.51" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2020</text>
    <text x="179.59" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2025</text>
    <text x="211.64" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2030</text>
    <text x="243.7" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2035</text>
    <text x="-108.98" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">Jan 1, 1980</text>
    <text x="-44.85" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">1990</text>
    <text x="19.27" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2000</text>
    <text x="83.4" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2010</text>
    <text x="147.51" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2020</text>
    <text x="211.64" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2030</text>
    <text x="-108.98" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">Jan 1, 1980</text>
    <text x="-44.85" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">1990</text>
    <text x="19.27" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2000</text>
    <text x="83.4" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2010</text>
    <text x="147.51" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2020</text>
    <text x="211.64" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2030</text>
    <text x="-108.98" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">Jan 1, 1980</text>
    <text x="-44.85" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">1990</text>
    <text x="19.27" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2000</text>
    <text x="83.4" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2010</text>
    <text x="147.51" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2020</text>
    <text x="211.64" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2030</text>
  </g>
  <g class="guide colorkey" id="img-9020955a-4">
    <g fill="#4C404B" font-size="2.82" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" id="img-9020955a-5">
      <text x="121.27" y="41.04" dy="0.35em" id="img-9020955a-6" class="color_Emission">Emission</text>
      <text x="121.27" y="44.67" dy="0.35em" id="img-9020955a-7" class="color_SoilMoisture">SoilMoisture</text>
      <text x="121.27" y="48.3" dy="0.35em" id="img-9020955a-8" class="color_t2m">t2m</text>
    </g>
    <g stroke="#000000" stroke-opacity="0.000" id="img-9020955a-9">
      <rect x="118.45" y="40.14" width="1.81" height="1.81" id="img-9020955a-10" class="color_Emission" fill="#00BFFF"/>
      <rect x="118.45" y="43.76" width="1.81" height="1.81" id="img-9020955a-11" class="color_SoilMoisture" fill="#D4CA3A"/>
      <rect x="118.45" y="47.39" width="1.81" height="1.81" id="img-9020955a-12" class="color_t2m" fill="#FF6DAE"/>
    </g>
    <g fill="#362A35" font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" stroke="#000000" stroke-opacity="0.000" id="img-9020955a-13">
      <text x="118.45" y="37.22" id="img-9020955a-14">Color</text>
    </g>
  </g>
<g clip-path="url(#img-9020955a-15)">
  <g id="img-9020955a-16">
    <g pointer-events="visible" opacity="1" fill="#000000" fill-opacity="0.000" stroke="#000000" stroke-opacity="0.000" class="guide background" id="img-9020955a-17">
      <rect x="17.27" y="5" width="100.19" height="75.72" id="img-9020955a-18"/>
    </g>
    <g class="guide ygridlines xfixed" stroke-dasharray="0.5,0.5" stroke-width="0.2" stroke="#D0D0E0" id="img-9020955a-19">
      <path fill="none" d="M17.27,174.34 L 117.45 174.34" id="img-9020955a-20" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,150.43 L 117.45 150.43" id="img-9020955a-21" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,126.52 L 117.45 126.52" id="img-9020955a-22" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,102.62 L 117.45 102.62" id="img-9020955a-23" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,78.71 L 117.45 78.71" id="img-9020955a-24" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,54.81 L 117.45 54.81" id="img-9020955a-25" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,30.9 L 117.45 30.9" id="img-9020955a-26" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,7 L 117.45 7" id="img-9020955a-27" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,-16.9 L 117.45 -16.9" id="img-9020955a-28" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-40.81 L 117.45 -40.81" id="img-9020955a-29" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-64.72 L 117.45 -64.72" id="img-9020955a-30" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-88.62 L 117.45 -88.62" id="img-9020955a-31" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,150.43 L 117.45 150.43" id="img-9020955a-32" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,148.04 L 117.45 148.04" id="img-9020955a-33" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,145.65 L 117.45 145.65" id="img-9020955a-34" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,143.26 L 117.45 143.26" id="img-9020955a-35" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,140.87 L 117.45 140.87" id="img-9020955a-36" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,138.48 L 117.45 138.48" id="img-9020955a-37" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,136.09 L 117.45 136.09" id="img-9020955a-38" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,133.7 L 117.45 133.7" id="img-9020955a-39" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,131.31 L 117.45 131.31" id="img-9020955a-40" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,128.92 L 117.45 128.92" id="img-9020955a-41" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,126.52 L 117.45 126.52" id="img-9020955a-42" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,124.13 L 117.45 124.13" id="img-9020955a-43" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,121.74 L 117.45 121.74" id="img-9020955a-44" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,119.35 L 117.45 119.35" id="img-9020955a-45" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,116.96 L 117.45 116.96" id="img-9020955a-46" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,114.57 L 117.45 114.57" id="img-9020955a-47" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,112.18 L 117.45 112.18" id="img-9020955a-48" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,109.79 L 117.45 109.79" id="img-9020955a-49" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,107.4 L 117.45 107.4" id="img-9020955a-50" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,105.01 L 117.45 105.01" id="img-9020955a-51" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,102.62 L 117.45 102.62" id="img-9020955a-52" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,100.23 L 117.45 100.23" id="img-9020955a-53" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,97.84 L 117.45 97.84" id="img-9020955a-54" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,95.45 L 117.45 95.45" id="img-9020955a-55" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,93.06 L 117.45 93.06" id="img-9020955a-56" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,90.67 L 117.45 90.67" id="img-9020955a-57" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,88.28 L 117.45 88.28" id="img-9020955a-58" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,85.89 L 117.45 85.89" id="img-9020955a-59" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,83.5 L 117.45 83.5" id="img-9020955a-60" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,81.11 L 117.45 81.11" id="img-9020955a-61" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,78.71 L 117.45 78.71" id="img-9020955a-62" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,76.32 L 117.45 76.32" id="img-9020955a-63" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,73.93 L 117.45 73.93" id="img-9020955a-64" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,71.54 L 117.45 71.54" id="img-9020955a-65" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,69.15 L 117.45 69.15" id="img-9020955a-66" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,66.76 L 117.45 66.76" id="img-9020955a-67" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,64.37 L 117.45 64.37" id="img-9020955a-68" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,61.98 L 117.45 61.98" id="img-9020955a-69" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,59.59 L 117.45 59.59" id="img-9020955a-70" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,57.2 L 117.45 57.2" id="img-9020955a-71" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,54.81 L 117.45 54.81" id="img-9020955a-72" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,52.42 L 117.45 52.42" id="img-9020955a-73" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,50.03 L 117.45 50.03" id="img-9020955a-74" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,47.64 L 117.45 47.64" id="img-9020955a-75" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,45.25 L 117.45 45.25" id="img-9020955a-76" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,42.86 L 117.45 42.86" id="img-9020955a-77" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,40.47 L 117.45 40.47" id="img-9020955a-78" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,38.08 L 117.45 38.08" id="img-9020955a-79" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,35.69 L 117.45 35.69" id="img-9020955a-80" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,33.3 L 117.45 33.3" id="img-9020955a-81" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,30.9 L 117.45 30.9" id="img-9020955a-82" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,28.51 L 117.45 28.51" id="img-9020955a-83" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,26.12 L 117.45 26.12" id="img-9020955a-84" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,23.73 L 117.45 23.73" id="img-9020955a-85" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,21.34 L 117.45 21.34" id="img-9020955a-86" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,18.95 L 117.45 18.95" id="img-9020955a-87" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,16.56 L 117.45 16.56" id="img-9020955a-88" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,14.17 L 117.45 14.17" id="img-9020955a-89" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,11.78 L 117.45 11.78" id="img-9020955a-90" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,9.39 L 117.45 9.39" id="img-9020955a-91" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,7 L 117.45 7" id="img-9020955a-92" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,4.61 L 117.45 4.61" id="img-9020955a-93" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,2.22 L 117.45 2.22" id="img-9020955a-94" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-0.17 L 117.45 -0.17" id="img-9020955a-95" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-2.56 L 117.45 -2.56" id="img-9020955a-96" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-4.95 L 117.45 -4.95" id="img-9020955a-97" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-7.34 L 117.45 -7.34" id="img-9020955a-98" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-9.73 L 117.45 -9.73" id="img-9020955a-99" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-12.12 L 117.45 -12.12" id="img-9020955a-100" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-14.51 L 117.45 -14.51" id="img-9020955a-101" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-16.9 L 117.45 -16.9" id="img-9020955a-102" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-19.3 L 117.45 -19.3" id="img-9020955a-103" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-21.69 L 117.45 -21.69" id="img-9020955a-104" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-24.08 L 117.45 -24.08" id="img-9020955a-105" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-26.47 L 117.45 -26.47" id="img-9020955a-106" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-28.86 L 117.45 -28.86" id="img-9020955a-107" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-31.25 L 117.45 -31.25" id="img-9020955a-108" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-33.64 L 117.45 -33.64" id="img-9020955a-109" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-36.03 L 117.45 -36.03" id="img-9020955a-110" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-38.42 L 117.45 -38.42" id="img-9020955a-111" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-40.81 L 117.45 -40.81" id="img-9020955a-112" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-43.2 L 117.45 -43.2" id="img-9020955a-113" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-45.59 L 117.45 -45.59" id="img-9020955a-114" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-47.98 L 117.45 -47.98" id="img-9020955a-115" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-50.37 L 117.45 -50.37" id="img-9020955a-116" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-52.76 L 117.45 -52.76" id="img-9020955a-117" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-55.15 L 117.45 -55.15" id="img-9020955a-118" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-57.54 L 117.45 -57.54" id="img-9020955a-119" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-59.93 L 117.45 -59.93" id="img-9020955a-120" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-62.32 L 117.45 -62.32" id="img-9020955a-121" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-64.72 L 117.45 -64.72" id="img-9020955a-122" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,222.14 L 117.45 222.14" id="img-9020955a-123" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.27,126.52 L 117.45 126.52" id="img-9020955a-124" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.27,30.9 L 117.45 30.9" id="img-9020955a-125" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.27,-64.72 L 117.45 -64.72" id="img-9020955a-126" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.27,150.43 L 117.45 150.43" id="img-9020955a-127" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,145.65 L 117.45 145.65" id="img-9020955a-128" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,140.87 L 117.45 140.87" id="img-9020955a-129" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,136.09 L 117.45 136.09" id="img-9020955a-130" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,131.31 L 117.45 131.31" id="img-9020955a-131" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,126.52 L 117.45 126.52" id="img-9020955a-132" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,121.74 L 117.45 121.74" id="img-9020955a-133" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,116.96 L 117.45 116.96" id="img-9020955a-134" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,112.18 L 117.45 112.18" id="img-9020955a-135" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,107.4 L 117.45 107.4" id="img-9020955a-136" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,102.62 L 117.45 102.62" id="img-9020955a-137" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,97.84 L 117.45 97.84" id="img-9020955a-138" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,93.06 L 117.45 93.06" id="img-9020955a-139" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,88.28 L 117.45 88.28" id="img-9020955a-140" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,83.5 L 117.45 83.5" id="img-9020955a-141" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,78.71 L 117.45 78.71" id="img-9020955a-142" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,73.93 L 117.45 73.93" id="img-9020955a-143" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,69.15 L 117.45 69.15" id="img-9020955a-144" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,64.37 L 117.45 64.37" id="img-9020955a-145" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,59.59 L 117.45 59.59" id="img-9020955a-146" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,54.81 L 117.45 54.81" id="img-9020955a-147" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,50.03 L 117.45 50.03" id="img-9020955a-148" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,45.25 L 117.45 45.25" id="img-9020955a-149" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,40.47 L 117.45 40.47" id="img-9020955a-150" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,35.69 L 117.45 35.69" id="img-9020955a-151" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,30.9 L 117.45 30.9" id="img-9020955a-152" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,26.12 L 117.45 26.12" id="img-9020955a-153" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,21.34 L 117.45 21.34" id="img-9020955a-154" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,16.56 L 117.45 16.56" id="img-9020955a-155" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,11.78 L 117.45 11.78" id="img-9020955a-156" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,7 L 117.45 7" id="img-9020955a-157" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,2.22 L 117.45 2.22" id="img-9020955a-158" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-2.56 L 117.45 -2.56" id="img-9020955a-159" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-7.34 L 117.45 -7.34" id="img-9020955a-160" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-12.12 L 117.45 -12.12" id="img-9020955a-161" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-16.9 L 117.45 -16.9" id="img-9020955a-162" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-21.69 L 117.45 -21.69" id="img-9020955a-163" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-26.47 L 117.45 -26.47" id="img-9020955a-164" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-31.25 L 117.45 -31.25" id="img-9020955a-165" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-36.03 L 117.45 -36.03" id="img-9020955a-166" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-40.81 L 117.45 -40.81" id="img-9020955a-167" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-45.59 L 117.45 -45.59" id="img-9020955a-168" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-50.37 L 117.45 -50.37" id="img-9020955a-169" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-55.15 L 117.45 -55.15" id="img-9020955a-170" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-59.93 L 117.45 -59.93" id="img-9020955a-171" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-64.72 L 117.45 -64.72" id="img-9020955a-172" gadfly:scale="5.0" visibility="hidden"/>
    </g>
    <g class="guide xgridlines yfixed" stroke-dasharray="0.5,0.5" stroke-width="0.2" stroke="#D0D0E0" id="img-9020955a-173">
      <path fill="none" d="M-108.98,5 L -108.98 80.72" id="img-9020955a-174" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-76.91,5 L -76.91 80.72" id="img-9020955a-175" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-44.85,5 L -44.85 80.72" id="img-9020955a-176" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-12.79,5 L -12.79 80.72" id="img-9020955a-177" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M19.27,5 L 19.27 80.72" id="img-9020955a-178" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M51.34,5 L 51.34 80.72" id="img-9020955a-179" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M83.4,5 L 83.4 80.72" id="img-9020955a-180" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M115.45,5 L 115.45 80.72" id="img-9020955a-181" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M147.51,5 L 147.51 80.72" id="img-9020955a-182" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M179.59,5 L 179.59 80.72" id="img-9020955a-183" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M211.64,5 L 211.64 80.72" id="img-9020955a-184" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M243.7,5 L 243.7 80.72" id="img-9020955a-185" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-108.98,5 L -108.98 80.72" id="img-9020955a-186" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M-44.85,5 L -44.85 80.72" id="img-9020955a-187" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M19.27,5 L 19.27 80.72" id="img-9020955a-188" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M83.4,5 L 83.4 80.72" id="img-9020955a-189" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M147.51,5 L 147.51 80.72" id="img-9020955a-190" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M211.64,5 L 211.64 80.72" id="img-9020955a-191" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M-108.98,5 L -108.98 80.72" id="img-9020955a-192" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M-44.85,5 L -44.85 80.72" id="img-9020955a-193" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M19.27,5 L 19.27 80.72" id="img-9020955a-194" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M83.4,5 L 83.4 80.72" id="img-9020955a-195" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M147.51,5 L 147.51 80.72" id="img-9020955a-196" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M211.64,5 L 211.64 80.72" id="img-9020955a-197" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M-108.98,5 L -108.98 80.72" id="img-9020955a-198" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M-44.85,5 L -44.85 80.72" id="img-9020955a-199" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M19.27,5 L 19.27 80.72" id="img-9020955a-200" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M83.4,5 L 83.4 80.72" id="img-9020955a-201" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M147.51,5 L 147.51 80.72" id="img-9020955a-202" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M211.64,5 L 211.64 80.72" id="img-9020955a-203" gadfly:scale="9.131666666666666" visibility="hidden"/>
    </g>
    <g class="plotpanel" id="img-9020955a-204">
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry color_t2m" stroke-dasharray="none" stroke="#FF6DAE" id="img-9020955a-205">
        <path fill="none" d="M19.27,30.9 L 19.41 30.9 19.55 30.91 19.69 30.91 19.83 30.9 19.97 30.91 20.11 30.91 20.25 30.91 20.39 30.9 20.53 30.9 20.67 30.9 20.81 30.91 20.95 30.91 21.09 30.91 21.23 30.9 21.37 30.9 21.51 30.91 21.65 30.9 21.79 30.9 21.93 30.9 22.07 30.91 22.22 30.9 22.36 30.9 22.5 30.9 22.64 30.91 22.78 30.9 22.92 30.9 23.06 30.91 23.2 30.91 23.34 30.9 23.48 30.91 23.62 30.9 23.76 30.9 23.9 30.91 24.04 30.91 24.18 30.9 24.32 30.9 24.46 30.91 24.6 30.9 24.74 30.91 24.88 30.91 25.02 30.91 25.16 30.9 25.31 30.91 25.45 30.9 25.59 30.9 25.69 26.95 25.83 30.4 25.97 27.43 26.11 30.27 26.25 26.77 26.39 26.42 26.53 26.32 26.67 28.12 26.81 19.21 26.96 25.79 27.1 34.08 27.24 27.7 27.38 28.01 27.52 26.74 27.66 29.2 27.8 37.22 27.94 32.87 28.08 31.96 28.22 31.11 28.36 27.97 28.5 25.5 28.64 28.73 28.78 27.31 28.92 28.18 29.06 26.55 29.2 28.52 29.34 28.01 29.48 29.29 29.62 25.4 29.76 31.04 29.9 32.46 30.05 34.23 30.19 31.17 30.33 35.64 30.47 32.53 30.61 35.51 30.75 31.11 30.89 39.09 31.03 40.76 31.17 37.56 31.31 25.29 31.45 29.84 31.59 33.74 31.73 41.82 31.87 37.87 32.01 25.14 32.1 24.91 32.24 27.89 32.38 35.16 32.52 31.25 32.66 35.22 32.8 41.98 32.94 38.27 33.08 30.6 33.22 28.14 33.36 18.87 33.5 22.9 33.64 30.42 33.78 33.6 33.93 29.85 34.07 27.36 34.21 26.26 34.35 28.65 34.49 28.29 34.63 29.37 34.77 30.42 34.91 34.07 35.05 29.69 35.19 31.72 35.33 32.11 35.47 32.68 35.61 29.47 35.75 30.86 35.89 31.72 36.03 29.72 36.17 32.58 36.31 30.45 36.45 31.32 36.59 35.62 36.73 28.88 36.87 25.65 37.01 25.21 37.16 21.72 37.3 22.54 37.44 17.66 37.58 18.02 37.72 23.57 37.86 22.59 38 24.42 38.14 29.37 38.28 30.06 38.42 29.15 38.51 33.22 38.65 22.68 38.79 22.54 38.93 25.93 39.07 19.88 39.21 26.09 39.35 23.79 39.49 20.69 39.63 27.47 39.77 39.53 39.91 25 40.05 22.53 40.19 28.56 40.33 27.3 40.47 26.34 40.61 24.56 40.75 29.95 40.89 26.62 41.04 28.81 41.18 29.18 41.32 28.44 41.46 30.5 41.6 30.84 41.74 29.83 41.88 30.49 42.02 28.52 42.16 29.46 42.3 26.28 42.44 31.66 42.58 30.47 42.72 30.1 42.86 27.5 43 30.02 43.14 27.94 43.28 25.39 43.42 31.06 43.56 36.18 43.7 25.71 43.84 17.36 43.98 25.6 44.13 23.92 44.27 36.49 44.41 35.35 44.55 27.48 44.69 33.27 44.83 23.28 44.92 28.68 45.06 40.04 45.2 31.44 45.34 26.47 45.48 26.72 45.62 29.08 45.76 30.5 45.9 26.4 46.04 29.05 46.18 28.59 46.32 23.89 46.46 42.17 46.6 27.4 46.74 27.93 46.88 28.04 47.02 27.61 47.16 28.15 47.3 27.47 47.44 28.43 47.58 29.91 47.72 31.86 47.86 29.99 48 29.73 48.15 28.1 48.29 26.97 48.43 24.04 48.57 26.84 48.71 25.49 48.85 25.04 48.99 22.83 49.13 25.97 49.27 25.42 49.41 24.09 49.55 27.31 49.69 26.25 49.83 26 49.97 24.24 50.11 25.86 50.25 34.34 50.39 24.28 50.53 23.78 50.67 22.6 50.81 32.88 50.95 24.01 51.09 25.16 51.24 25.55 51.34 20.41 51.48 21.58 51.62 22.41 51.76 22.39 51.9 27.14 52.04 27.85 52.18 24.31 52.32 25.95 52.46 18.8 52.6 15.83 52.74 23.05 52.89 38.12 53.03 30.78 53.17 24.86 53.31 23.15 53.45 23.73 53.59 22.45 53.73 26.93 53.87 27.35 54.01 26.07 54.15 22.98 54.29 25.7 54.43 24.85 54.57 24.82 54.71 24.79 54.85 26.6 54.99 23.89 55.13 25.03 55.27 24.26 55.41 24.83 55.55 28.16 55.69 26.65 55.83 27.64 55.98 27.08 56.12 32.35 56.26 31.18 56.4 28.99 56.54 35.83 56.68 27.68 56.82 38.37 56.96 41.3 57.1 35.01 57.24 24.41 57.38 22.56 57.52 22.79 57.66 25.61 57.75 25.22 57.89 30.62 58.03 39.15 58.17 52.23 58.31 39.17 58.45 27.24 58.59 28.69 58.73 36.21 58.87 27.93 59.01 23.42 59.15 27.02 59.29 23.47 59.43 28.52 59.57 37.95 59.71 39.34 59.86 32.81 60 32.4 60.14 32.25 60.28 33.59 60.42 29.21 60.56 31.14 60.7 30.92 60.84 31.04 60.98 31.89 61.12 33.29 61.26 34 61.4 34.08 61.54 36.96 61.68 36 61.82 32.6 61.96 31.84 62.1 33.24 62.24 32.88 62.38 34.34 62.52 28.16 62.66 27.48 62.8 28.29 62.94 31.65 63.09 35.5 63.23 39.64 63.37 40.77 63.51 29.53 63.65 32.48 63.79 34.84 63.93 40.13 64.07 36.77 64.16 42.4 64.3 29.38 64.44 37.37 64.58 23.73 64.72 25.15 64.86 29.35 65 35.42 65.14 39.18 65.28 59.73 65.42 40.69 65.56 40.07 65.7 28.95 65.84 31.66 65.98 33.62 66.12 33.38 66.26 31.76 66.4 33.23 66.54 32.87 66.68 35.23 66.82 33.68 66.97 32.71 67.11 30.03 67.25 30.16 67.39 27.86 67.53 32.82 67.67 30.02 67.81 30.93 67.95 29.3 68.09 28.94 68.23 28.53 68.37 27.8 68.51 27.74 68.65 28.26 68.79 29.77 68.93 31.96 69.07 35.82 69.21 39.8 69.35 28.29 69.49 29.75 69.63 32.87 69.77 31.39 69.91 23.81 70.06 30.6 70.2 33.3 70.34 33.71 70.48 31.4 70.56 47.69 70.7 50.33 70.85 28.13 70.99 30.54 71.13 32.73 71.27 36.57 71.41 38.49 71.55 32.33 71.69 29.58 71.83 33.76 71.97 51.39 72.11 26.83 72.25 34.06 72.39 32.31 72.53 37.26 72.67 40.28 72.81 35.24 72.95 35.28 73.09 33.17 73.23 35.61 73.37 35.99 73.51 33.51 73.65 33.74 73.79 36.05 73.93 32.41 74.08 34.85 74.22 32.36 74.36 30.39 74.5 32.72 74.64 34.27 74.78 36.01 74.92 36.65 75.06 36.87 75.2 33.31 75.34 36.69 75.48 32.91 75.62 36.73 75.76 35.42 75.9 35.64 76.04 32.02 76.18 31.57 76.32 39.31 76.46 29.08 76.6 24.7 76.74 24.18 76.88 51.52 76.99 38.52 77.13 27.27 77.27 28.27 77.41 37.49 77.55 38.1 77.69 32.87 77.83 31.76 77.97 28.36 78.11 26.79 78.25 51.4 78.39 33.69 78.53 31.61 78.68 34.76 78.82 34.51 78.96 27.9 79.1 29.07 79.24 32.36 79.38 30.3 79.52 28.28 79.66 31.9 79.8 32.22 79.94 33.45 80.08 32.5 80.22 30.49 80.36 29.8 80.5 34.22 80.64 35.85 80.78 37.64 80.92 37.6 81.06 36.87 81.2 35.33 81.34 34.55 81.48 32.66 81.62 34.32 81.76 29.82 81.91 26.69 82.05 29.7 82.19 34.93 82.33 34.12 82.47 33.07 82.61 42.46 82.75 39 82.89 28.84 83.03 35.39 83.17 22.12 83.31 24.37 83.4 21.04 83.54 28.88 83.68 37.15 83.82 28.77 83.96 38.18 84.1 31.59 84.24 31.51 84.38 41.21 84.52 42.35 84.66 31.16 84.8 27.97 84.94 37.24 85.08 31.71 85.22 33.98 85.36 37.09 85.5 35.75 85.64 33.73 85.79 37.09 85.93 33.71 86.07 35.1 86.21 34.16 86.35 36.53 86.49 37.16 86.63 39.71 86.77 39.25 86.91 38.81 87.05 36.77 87.19 36.95 87.33 37.7 87.47 35.04 87.61 30.93 87.75 31.75 87.89 29.82 88.03 30.46 88.17 40.26 88.31 37.19 88.45 32.29 88.59 29.71 88.73 36.25 88.88 27.62 89.02 24.99 89.16 30.87 89.3 37.25 89.44 35.57 89.58 39.77 89.72 36.26" id="img-9020955a-206"/>
      </g>
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry color_SoilMoisture" stroke-dasharray="none" stroke="#D4CA3A" id="img-9020955a-207">
        <path fill="none" d="M19.27,30.9 L 19.41 30.9 19.55 30.9 19.69 30.9 19.83 30.91 19.97 30.91 20.11 30.9 20.25 30.91 20.39 30.9 20.53 30.91 20.67 30.9 20.81 30.16 20.95 30.13 21.09 30.91 21.23 31.62 21.37 30.67 21.51 31 21.65 31.16 21.79 30.79 21.93 31.17 22.07 30.87 22.22 30.98 22.36 31.04 22.5 30.96 22.64 31.14 22.78 30.95 22.92 30.85 23.06 30.82 23.2 31.04 23.34 30.58 23.48 30.82 23.62 30.86 23.76 29.87 23.9 30.84 24.04 31.23 24.18 31.15 24.32 31.03 24.46 30.69 24.6 30.63 24.74 30.91 24.88 30.91 25.02 30.91 25.16 30.91 25.31 30.91 25.45 30.91 25.59 30.9 25.69 30.9 25.83 30.9 25.97 30.9 26.11 30.9 26.25 30.91 26.39 30.91 26.53 30.9 26.67 30.91 26.81 30.9 26.96 30.91 27.1 30.9 27.24 31.14 27.38 31.88 27.52 30.75 27.66 30.91 27.8 30.76 27.94 31.05 28.08 30.9 28.22 30.75 28.36 30.91 28.5 30.9 28.64 30.91 28.78 30.91 28.92 30.62 29.06 30.73 29.2 30.68 29.34 30.5 29.48 30.31 29.62 30.57 29.76 30.64 29.9 30.78 30.05 30.71 30.19 30.63 30.33 30.94 30.47 30.66 30.61 31.66 30.75 30.91 30.89 30.89 31.03 30.97 31.17 32.03 31.31 30.91 31.45 30.91 31.59 30.91 31.73 30.91 31.87 30.91 32.01 30.9 32.1 30.9 32.24 30.9 32.38 30.9 32.52 30.9 32.66 30.91 32.8 30.91 32.94 30.9 33.08 30.91 33.22 30.9 33.36 30.91 33.5 30.9 33.64 31.29 33.78 31.09 33.93 30.71 34.07 30.7 34.21 30.97 34.35 31.17 34.49 30.64 34.63 30.28 34.77 30.71 34.91 30.76 35.05 31.15 35.19 31.22 35.33 30.8 35.47 31.03 35.61 31.1 35.75 31.15 35.89 31.12 36.03 30.94 36.17 31.25 36.31 31.23 36.45 31.21 36.59 31.33 36.73 30.96 36.87 31.15 37.01 30.59 37.16 30.65 37.3 30.77 37.44 30.91 37.58 30.93 37.72 31 37.86 31.09 38 31.59 38.14 31.61 38.28 31.53 38.42 31.3 38.51 30.9 38.65 31.03 38.79 31.14 38.93 30.61 39.07 31.37 39.21 31.09 39.35 31.11 39.49 30.91 39.63 31.21 39.77 30.94 39.91 31.22 40.05 31.09 40.19 31.1 40.33 30.99 40.47 30.88 40.61 30.99 40.75 30.93 40.89 31 41.04 31.17 41.18 31.22 41.32 31.29 41.46 30.96 41.6 30.93 41.74 30.75 41.88 30.81 42.02 30.9 42.16 31.13 42.3 31.24 42.44 31.2 42.58 30.94 42.72 30.87 42.86 31 43 31.23 43.14 31.07 43.28 30.72 43.42 30.65 43.56 30.89 43.7 30.97 43.84 30.67 43.98 30.89 44.13 30.88 44.27 30.92 44.41 30.9 44.55 31.01 44.69 30.8 44.83 30.86 44.92 30.8 45.06 31.13 45.2 30.72 45.34 30.48 45.48 30.48 45.62 30.7 45.76 30.89 45.9 30.91 46.04 31.22 46.18 30.63 46.32 30.78 46.46 30.93 46.6 30.88 46.74 30.91 46.88 30.81 47.02 30.89 47.16 30.82 47.3 30.93 47.44 30.92 47.58 31.01 47.72 30.92 47.86 30.88 48 30.97 48.15 31.05 48.29 30.57 48.43 29.88 48.57 30.54 48.71 31.03 48.85 31.01 48.99 30.99 49.13 31.05 49.27 31.13 49.41 31.19 49.55 30.92 49.69 30.96 49.83 30.78 49.97 30.83 50.11 30.92 50.25 30.93 50.39 30.68 50.53 31.12 50.67 31.16 50.81 30.77 50.95 30.71 51.09 30.95 51.24 30.51 51.34 30.7 51.48 30.69 51.62 31.17 51.76 31.03 51.9 31.44 52.04 31.26 52.18 31.18 52.32 30.91 52.46 31.63 52.6 31.3 52.74 30.6 52.89 31 53.03 30.82 53.17 31.05 53.31 30.85 53.45 30.87 53.59 30.62 53.73 30.62 53.87 31.03 54.01 30.75 54.15 30.76 54.29 30.77 54.43 30.39 54.57 30.36 54.71 31.13 54.85 31.03 54.99 30.77 55.13 30.82 55.27 31.02 55.41 31.14 55.55 31.3 55.69 31.15 55.83 30.79 55.98 30.91 56.12 30.74 56.26 30.88 56.4 31.06 56.54 30.92 56.68 31.01 56.82 30.6 56.96 31.16 57.1 31.18 57.24 30.72 57.38 30.5 57.52 30.99 57.66 31.26 57.75 31.6 57.89 31.3 58.03 31.36 58.17 31.29 58.31 31.63 58.45 31.68 58.59 31.19 58.73 30.91 58.87 30.9 59.01 31.42 59.15 31.42 59.29 30.78 59.43 30.86 59.57 30.91 59.71 30.81 59.86 31.13 60 31 60.14 30.84 60.28 30.74 60.42 30.67 60.56 30.87 60.7 30.66 60.84 30.61 60.98 30.75 61.12 30.83 61.26 31.23 61.4 31.17 61.54 30.86 61.68 30.74 61.82 30.82 61.96 30.72 62.1 30.85 62.24 31.04 62.38 31.01 62.52 30.83 62.66 30.83 62.8 30.87 62.94 31 63.09 30.98 63.23 30.69 63.37 30.98 63.51 30.78 63.65 30.96 63.79 30.68 63.93 30.5 64.07 30.59 64.16 30.51 64.3 30.37 64.44 30.72 64.58 30.72 64.72 30.74 64.86 30.91 65 30.9 65.14 30.76 65.28 30.33 65.42 30.41 65.56 30.71 65.7 31.45 65.84 30.79 65.98 30.94 66.12 31.04 66.26 31.17 66.4 30.74 66.54 31.17 66.68 31.2 66.82 30.92 66.97 31.02 67.11 30.77 67.25 30.94 67.39 30.95 67.53 30.85 67.67 31.15 67.81 30.8 67.95 30.81 68.09 30.94 68.23 30.91 68.37 30.86 68.51 30.89 68.65 31.01 68.79 30.82 68.93 31.06 69.07 30.74 69.21 30.96 69.35 31.33 69.49 30.95 69.63 30.64 69.77 30.91 69.91 30.67 70.06 30.82 70.2 30.91 70.34 30.91 70.48 30.9 70.56 30.9 70.7 30.9 70.85 30.36 70.99 31.63 71.13 30.73 71.27 30.24 71.41 30.31 71.55 30.67 71.69 30.37 71.83 30.75 71.97 31.09 72.11 30.74 72.25 30.83 72.39 30.65 72.53 30.69 72.67 30.73 72.81 30.82 72.95 30.97 73.09 31.23 73.23 31.12 73.37 31.07 73.51 31.06 73.65 31.02 73.79 31.09 73.93 30.92 74.08 31.13 74.22 31.17 74.36 30.98 74.5 30.71 74.64 30.92 74.78 30.93 74.92 30.79 75.06 31.06 75.2 30.68 75.34 30.95 75.48 30.96 75.62 30.99 75.76 31 75.9 31.16 76.04 31.28 76.18 30.75 76.32 30.85 76.46 30.62 76.6 30.44 76.74 30.66 76.88 30.9 76.99 30.9 77.13 30.9 77.27 30.87 77.41 30.57 77.55 29.95 77.69 30.46 77.83 30.9 77.97 30.91 78.11 30.4 78.25 30.65 78.39 30.78 78.53 30.89 78.68 31.02 78.82 31.13 78.96 31.11 79.1 31.17 79.24 31.21 79.38 31.14 79.52 31.03 79.66 30.81 79.8 30.89 79.94 30.9 80.08 30.79 80.22 31.24 80.36 30.84 80.5 30.76 80.64 31.11 80.78 31.14 80.92 30.81 81.06 30.97 81.2 30.77 81.34 30.77 81.48 31.13 81.62 31.01 81.76 30.92 81.91 30.8 82.05 30.78 82.19 30.69 82.33 31.12 82.47 30.69 82.61 30.88 82.75 31.1 82.89 30.86 83.03 31.37 83.17 30.91 83.31 30.9 83.4 30.9 83.54 30.9 83.68 30.9 83.82 30.9 83.96 30.91 84.1 30.91 84.24 30.74 84.38 31.28 84.52 31.19 84.66 31.16 84.8 30.64 84.94 30.48 85.08 30.55 85.22 31.01 85.36 30.55 85.5 30.6 85.64 30.61 85.79 30.58 85.93 30.82 86.07 30.68 86.21 30.59 86.35 30.93 86.49 31.13 86.63 31.4 86.77 31.11 86.91 31.15 87.05 30.76 87.19 30.81 87.33 30.98 87.47 30.8 87.61 30.63 87.75 30.6 87.89 30.67 88.03 30.78 88.17 30.73 88.31 30.91 88.45 31.01 88.59 30.79 88.73 30.62 88.88 30.61 89.02 30.47 89.16 30.4 89.3 30.91 89.44 30.91 89.58 30.91 89.72 30.9" id="img-9020955a-208"/>
      </g>
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry color_Emission" stroke-dasharray="none" stroke="#00BFFF" id="img-9020955a-209">
        <path fill="none" d="M19.27,30.9 L 19.41 30.9 19.55 30.9 19.69 30.9 19.83 30.9 19.97 30.9 20.11 30.9 20.25 30.9 20.39 30.9 20.53 30.9 20.67 30.9 20.81 30.9 20.95 30.9 21.09 30.9 21.23 30.9 21.37 30.9 21.51 30.9 21.65 30.9 21.79 30.9 21.93 30.9 22.07 30.9 22.22 30.9 22.36 30.9 22.5 30.9 22.64 30.9 22.78 30.9 22.92 30.9 23.06 30.9 23.2 30.9 23.34 30.9 23.48 30.9 23.62 30.9 23.76 30.9 23.9 30.9 24.04 30.9 24.18 30.9 24.32 30.9 24.46 30.9 24.6 30.9 24.74 30.9 24.88 30.9 25.02 30.9 25.16 30.9 25.31 30.9 25.45 30.9 25.59 30.9 25.69 30.9 25.83 30.9 25.97 30.9 26.11 30.9 26.25 30.9 26.39 30.9 26.53 30.9 26.67 30.9 26.81 30.9 26.96 30.9 27.1 30.9 27.24 30.97 27.38 31.08 27.52 31.08 27.66 31.08 27.8 30.9 27.94 30.9 28.08 30.9 28.22 30.9 28.36 30.9 28.5 30.9 28.64 30.9 28.78 30.9 28.92 30.9 29.06 30.9 29.2 30.9 29.34 30.9 29.48 30.9 29.62 30.9 29.76 30.9 29.9 30.9 30.05 30.9 30.19 30.9 30.33 30.9 30.47 30.9 30.61 30.9 30.75 30.9 30.89 30.9 31.03 30.9 31.17 30.9 31.31 30.9 31.45 30.9 31.59 30.9 31.73 30.9 31.87 30.9 32.01 30.9 32.1 30.9 32.24 30.9 32.38 30.9 32.52 30.9 32.66 30.9 32.8 30.9 32.94 30.9 33.08 30.9 33.22 30.9 33.36 30.9 33.5 30.9 33.64 30.97 33.78 31.08 33.93 31.08 34.07 31.08 34.21 30.9 34.35 30.9 34.49 30.9 34.63 30.9 34.77 30.9 34.91 30.9 35.05 30.9 35.19 30.9 35.33 30.9 35.47 30.9 35.61 30.9 35.75 30.9 35.89 30.9 36.03 30.9 36.17 30.9 36.31 30.9 36.45 30.9 36.59 30.9 36.73 30.9 36.87 30.9 37.01 30.9 37.16 30.9 37.3 30.9 37.44 30.9 37.58 30.9 37.72 30.9 37.86 30.9 38 30.9 38.14 30.9 38.28 30.9 38.42 30.9 38.51 30.9 38.65 30.9 38.79 30.9 38.93 30.9 39.07 30.9 39.21 30.9 39.35 30.9 39.49 30.9 39.63 30.9 39.77 30.9 39.91 30.9 40.05 30.97 40.19 31.08 40.33 31.08 40.47 31.08 40.61 30.9 40.75 30.9 40.89 30.9 41.04 30.9 41.18 30.9 41.32 30.9 41.46 30.9 41.6 30.9 41.74 30.9 41.88 30.9 42.02 30.9 42.16 30.9 42.3 30.9 42.44 30.9 42.58 30.9 42.72 30.9 42.86 30.9 43 30.9 43.14 30.9 43.28 30.9 43.42 30.9 43.56 30.9 43.7 30.9 43.84 30.9 43.98 30.9 44.13 30.9 44.27 30.9 44.41 30.9 44.55 30.9 44.69 30.9 44.83 30.9 44.92 30.9 45.06 30.9 45.2 30.9 45.34 30.9 45.48 30.9 45.62 30.9 45.76 30.9 45.9 30.9 46.04 30.9 46.18 30.9 46.32 30.9 46.46 30.97 46.6 31.08 46.74 31.08 46.88 31.08 47.02 30.9 47.16 30.9 47.3 30.9 47.44 30.9 47.58 30.9 47.72 30.9 47.86 30.9 48 30.9 48.15 30.9 48.29 30.9 48.43 30.9 48.57 30.9 48.71 30.9 48.85 30.9 48.99 30.9 49.13 30.9 49.27 30.9 49.41 30.9 49.55 30.9 49.69 30.9 49.83 30.9 49.97 30.9 50.11 30.9 50.25 30.9 50.39 30.9 50.53 30.9 50.67 30.9 50.81 30.9 50.95 30.9 51.09 30.9 51.24 30.9 51.34 30.9 51.48 30.9 51.62 30.9 51.76 30.9 51.9 30.9 52.04 30.9 52.18 30.9 52.32 30.9 52.46 30.9 52.6 30.9 52.74 30.9 52.89 30.97 53.03 31.08 53.17 31.08 53.31 31.08 53.45 30.9 53.59 30.9 53.73 30.9 53.87 30.9 54.01 30.9 54.15 30.9 54.29 30.9 54.43 30.9 54.57 30.9 54.71 30.9 54.85 30.9 54.99 30.9 55.13 30.9 55.27 30.9 55.41 30.9 55.55 30.9 55.69 30.9 55.83 30.9 55.98 30.9 56.12 30.9 56.26 30.9 56.4 30.9 56.54 30.9 56.68 30.9 56.82 30.9 56.96 30.9 57.1 30.9 57.24 30.9 57.38 30.9 57.52 30.9 57.66 30.9 57.75 30.9 57.89 30.9 58.03 30.9 58.17 30.9 58.31 30.9 58.45 30.9 58.59 30.9 58.73 30.9 58.87 30.9 59.01 30.9 59.15 30.9 59.29 30.32 59.43 29.33 59.57 29.33 59.71 29.33 59.86 30.9 60 30.9 60.14 30.9 60.28 30.9 60.42 30.9 60.56 30.9 60.7 30.9 60.84 30.9 60.98 30.9 61.12 30.9 61.26 30.9 61.4 30.9 61.54 30.9 61.68 30.9 61.82 30.9 61.96 30.9 62.1 30.9 62.24 30.9 62.38 30.9 62.52 30.9 62.66 30.9 62.8 30.9 62.94 30.9 63.09 30.9 63.23 30.9 63.37 30.9 63.51 30.9 63.65 30.9 63.79 30.9 63.93 30.9 64.07 30.9 64.16 30.9 64.3 30.9 64.44 30.9 64.58 30.9 64.72 30.9 64.86 30.9 65 30.9 65.14 30.9 65.28 30.9 65.42 30.9 65.56 30.9 65.7 30.97 65.84 31.08 65.98 31.08 66.12 31.08 66.26 30.9 66.4 30.9 66.54 30.9 66.68 30.9 66.82 30.9 66.97 30.9 67.11 30.9 67.25 30.9 67.39 30.9 67.53 30.9 67.67 30.9 67.81 30.9 67.95 30.9 68.09 30.9 68.23 30.9 68.37 30.9 68.51 30.9 68.65 30.9 68.79 30.9 68.93 30.9 69.07 30.9 69.21 30.9 69.35 30.9 69.49 30.9 69.63 30.9 69.77 30.9 69.91 30.9 70.06 30.9 70.2 30.9 70.34 30.9 70.48 30.9 70.56 30.9 70.7 30.9 70.85 30.9 70.99 30.9 71.13 30.9 71.27 30.9 71.41 30.9 71.55 30.9 71.69 30.9 71.83 30.9 71.97 30.9 72.11 30.97 72.25 31.08 72.39 31.08 72.53 31.08 72.67 30.9 72.81 30.9 72.95 30.9 73.09 30.9 73.23 30.9 73.37 30.9 73.51 30.9 73.65 30.9 73.79 30.9 73.93 30.9 74.08 30.9 74.22 30.9 74.36 30.9 74.5 30.9 74.64 30.9 74.78 30.9 74.92 30.9 75.06 30.9 75.2 30.9 75.34 30.9 75.48 30.9 75.62 30.9 75.76 30.9 75.9 30.9 76.04 30.9 76.18 30.9 76.32 30.9 76.46 30.9 76.6 30.9 76.74 30.9 76.88 30.9 76.99 30.9 77.13 30.9 77.27 30.9 77.41 30.9 77.55 30.9 77.69 30.9 77.83 30.9 77.97 30.9 78.11 30.9 78.25 30.9 78.39 30.9 78.53 30.97 78.68 31.08 78.82 31.08 78.96 31.08 79.1 30.9 79.24 30.9 79.38 30.9 79.52 30.9 79.66 30.9 79.8 30.9 79.94 30.9 80.08 30.9 80.22 30.9 80.36 30.9 80.5 30.9 80.64 30.9 80.78 30.9 80.92 30.9 81.06 30.9 81.2 30.9 81.34 30.9 81.48 30.9 81.62 30.9 81.76 30.9 81.91 30.9 82.05 30.9 82.19 30.9 82.33 30.9 82.47 30.9 82.61 30.9 82.75 30.9 82.89 30.9 83.03 30.9 83.17 30.9 83.31 30.9 83.4 30.9 83.54 30.9 83.68 30.9 83.82 30.9 83.96 30.9 84.1 30.9 84.24 30.9 84.38 30.9 84.52 30.9 84.66 30.9 84.8 30.9 84.94 30.97 85.08 31.08 85.22 31.08 85.36 31.08 85.5 30.9 85.64 30.9 85.79 30.9 85.93 30.9 86.07 30.9 86.21 30.9 86.35 30.9 86.49 30.9 86.63 30.9 86.77 30.9 86.91 30.9 87.05 30.9 87.19 30.9 87.33 30.9 87.47 30.9 87.61 30.9 87.75 30.9 87.89 30.9 88.03 30.9 88.17 30.9 88.31 30.9 88.45 30.9 88.59 30.9 88.73 30.9 88.88 30.9 89.02 30.9 89.16 30.9 89.3 30.9 89.44 30.9 89.58 30.9 89.72 30.9" id="img-9020955a-210"/>
      </g>
    </g>
    <g opacity="0" class="guide zoomslider" stroke="#000000" stroke-opacity="0.000" id="img-9020955a-211">
      <g fill="#EAEAEA" stroke-width="0.3" stroke-opacity="0" stroke="#6A6A6A" id="img-9020955a-212">
        <rect x="110.45" y="8" width="4" height="4" id="img-9020955a-213"/>
        <g class="button_logo" fill="#6A6A6A" id="img-9020955a-214">
          <path d="M111.25,9.6 L 112.05 9.6 112.05 8.8 112.85 8.8 112.85 9.6 113.65 9.6 113.65 10.4 112.85 10.4 112.85 11.2 112.05 11.2 112.05 10.4 111.25 10.4 z" id="img-9020955a-215"/>
        </g>
      </g>
      <g fill="#EAEAEA" id="img-9020955a-216">
        <rect x="90.95" y="8" width="19" height="4" id="img-9020955a-217"/>
      </g>
      <g class="zoomslider_thumb" fill="#6A6A6A" id="img-9020955a-218">
        <rect x="99.45" y="8" width="2" height="4" id="img-9020955a-219"/>
      </g>
      <g fill="#EAEAEA" stroke-width="0.3" stroke-opacity="0" stroke="#6A6A6A" id="img-9020955a-220">
        <rect x="86.45" y="8" width="4" height="4" id="img-9020955a-221"/>
        <g class="button_logo" fill="#6A6A6A" id="img-9020955a-222">
          <path d="M87.25,9.6 L 89.65 9.6 89.65 10.4 87.25 10.4 z" id="img-9020955a-223"/>
        </g>
      </g>
    </g>
  </g>
</g>
  <g class="guide ylabels" font-size="2.82" font-family="'PT Sans Caption','Helvetica Neue','Helvetica',sans-serif" fill="#6C606B" id="img-9020955a-224">
    <text x="16.27" y="174.34" text-anchor="end" dy="0.35em" id="img-9020955a-225" gadfly:scale="1.0" visibility="hidden">-30</text>
    <text x="16.27" y="150.43" text-anchor="end" dy="0.35em" id="img-9020955a-226" gadfly:scale="1.0" visibility="hidden">-25</text>
    <text x="16.27" y="126.52" text-anchor="end" dy="0.35em" id="img-9020955a-227" gadfly:scale="1.0" visibility="hidden">-20</text>
    <text x="16.27" y="102.62" text-anchor="end" dy="0.35em" id="img-9020955a-228" gadfly:scale="1.0" visibility="hidden">-15</text>
    <text x="16.27" y="78.71" text-anchor="end" dy="0.35em" id="img-9020955a-229" gadfly:scale="1.0" visibility="visible">-10</text>
    <text x="16.27" y="54.81" text-anchor="end" dy="0.35em" id="img-9020955a-230" gadfly:scale="1.0" visibility="visible">-5</text>
    <text x="16.27" y="30.9" text-anchor="end" dy="0.35em" id="img-9020955a-231" gadfly:scale="1.0" visibility="visible">0</text>
    <text x="16.27" y="7" text-anchor="end" dy="0.35em" id="img-9020955a-232" gadfly:scale="1.0" visibility="visible">5</text>
    <text x="16.27" y="-16.9" text-anchor="end" dy="0.35em" id="img-9020955a-233" gadfly:scale="1.0" visibility="hidden">10</text>
    <text x="16.27" y="-40.81" text-anchor="end" dy="0.35em" id="img-9020955a-234" gadfly:scale="1.0" visibility="hidden">15</text>
    <text x="16.27" y="-64.72" text-anchor="end" dy="0.35em" id="img-9020955a-235" gadfly:scale="1.0" visibility="hidden">20</text>
    <text x="16.27" y="-88.62" text-anchor="end" dy="0.35em" id="img-9020955a-236" gadfly:scale="1.0" visibility="hidden">25</text>
    <text x="16.27" y="150.43" text-anchor="end" dy="0.35em" id="img-9020955a-237" gadfly:scale="10.0" visibility="hidden">-25.0</text>
    <text x="16.27" y="148.04" text-anchor="end" dy="0.35em" id="img-9020955a-238" gadfly:scale="10.0" visibility="hidden">-24.5</text>
    <text x="16.27" y="145.65" text-anchor="end" dy="0.35em" id="img-9020955a-239" gadfly:scale="10.0" visibility="hidden">-24.0</text>
    <text x="16.27" y="143.26" text-anchor="end" dy="0.35em" id="img-9020955a-240" gadfly:scale="10.0" visibility="hidden">-23.5</text>
    <text x="16.27" y="140.87" text-anchor="end" dy="0.35em" id="img-9020955a-241" gadfly:scale="10.0" visibility="hidden">-23.0</text>
    <text x="16.27" y="138.48" text-anchor="end" dy="0.35em" id="img-9020955a-242" gadfly:scale="10.0" visibility="hidden">-22.5</text>
    <text x="16.27" y="136.09" text-anchor="end" dy="0.35em" id="img-9020955a-243" gadfly:scale="10.0" visibility="hidden">-22.0</text>
    <text x="16.27" y="133.7" text-anchor="end" dy="0.35em" id="img-9020955a-244" gadfly:scale="10.0" visibility="hidden">-21.5</text>
    <text x="16.27" y="131.31" text-anchor="end" dy="0.35em" id="img-9020955a-245" gadfly:scale="10.0" visibility="hidden">-21.0</text>
    <text x="16.27" y="128.92" text-anchor="end" dy="0.35em" id="img-9020955a-246" gadfly:scale="10.0" visibility="hidden">-20.5</text>
    <text x="16.27" y="126.52" text-anchor="end" dy="0.35em" id="img-9020955a-247" gadfly:scale="10.0" visibility="hidden">-20.0</text>
    <text x="16.27" y="124.13" text-anchor="end" dy="0.35em" id="img-9020955a-248" gadfly:scale="10.0" visibility="hidden">-19.5</text>
    <text x="16.27" y="121.74" text-anchor="end" dy="0.35em" id="img-9020955a-249" gadfly:scale="10.0" visibility="hidden">-19.0</text>
    <text x="16.27" y="119.35" text-anchor="end" dy="0.35em" id="img-9020955a-250" gadfly:scale="10.0" visibility="hidden">-18.5</text>
    <text x="16.27" y="116.96" text-anchor="end" dy="0.35em" id="img-9020955a-251" gadfly:scale="10.0" visibility="hidden">-18.0</text>
    <text x="16.27" y="114.57" text-anchor="end" dy="0.35em" id="img-9020955a-252" gadfly:scale="10.0" visibility="hidden">-17.5</text>
    <text x="16.27" y="112.18" text-anchor="end" dy="0.35em" id="img-9020955a-253" gadfly:scale="10.0" visibility="hidden">-17.0</text>
    <text x="16.27" y="109.79" text-anchor="end" dy="0.35em" id="img-9020955a-254" gadfly:scale="10.0" visibility="hidden">-16.5</text>
    <text x="16.27" y="107.4" text-anchor="end" dy="0.35em" id="img-9020955a-255" gadfly:scale="10.0" visibility="hidden">-16.0</text>
    <text x="16.27" y="105.01" text-anchor="end" dy="0.35em" id="img-9020955a-256" gadfly:scale="10.0" visibility="hidden">-15.5</text>
    <text x="16.27" y="102.62" text-anchor="end" dy="0.35em" id="img-9020955a-257" gadfly:scale="10.0" visibility="hidden">-15.0</text>
    <text x="16.27" y="100.23" text-anchor="end" dy="0.35em" id="img-9020955a-258" gadfly:scale="10.0" visibility="hidden">-14.5</text>
    <text x="16.27" y="97.84" text-anchor="end" dy="0.35em" id="img-9020955a-259" gadfly:scale="10.0" visibility="hidden">-14.0</text>
    <text x="16.27" y="95.45" text-anchor="end" dy="0.35em" id="img-9020955a-260" gadfly:scale="10.0" visibility="hidden">-13.5</text>
    <text x="16.27" y="93.06" text-anchor="end" dy="0.35em" id="img-9020955a-261" gadfly:scale="10.0" visibility="hidden">-13.0</text>
    <text x="16.27" y="90.67" text-anchor="end" dy="0.35em" id="img-9020955a-262" gadfly:scale="10.0" visibility="hidden">-12.5</text>
    <text x="16.27" y="88.28" text-anchor="end" dy="0.35em" id="img-9020955a-263" gadfly:scale="10.0" visibility="hidden">-12.0</text>
    <text x="16.27" y="85.89" text-anchor="end" dy="0.35em" id="img-9020955a-264" gadfly:scale="10.0" visibility="hidden">-11.5</text>
    <text x="16.27" y="83.5" text-anchor="end" dy="0.35em" id="img-9020955a-265" gadfly:scale="10.0" visibility="hidden">-11.0</text>
    <text x="16.27" y="81.11" text-anchor="end" dy="0.35em" id="img-9020955a-266" gadfly:scale="10.0" visibility="hidden">-10.5</text>
    <text x="16.27" y="78.71" text-anchor="end" dy="0.35em" id="img-9020955a-267" gadfly:scale="10.0" visibility="hidden">-10.0</text>
    <text x="16.27" y="76.32" text-anchor="end" dy="0.35em" id="img-9020955a-268" gadfly:scale="10.0" visibility="hidden">-9.5</text>
    <text x="16.27" y="73.93" text-anchor="end" dy="0.35em" id="img-9020955a-269" gadfly:scale="10.0" visibility="hidden">-9.0</text>
    <text x="16.27" y="71.54" text-anchor="end" dy="0.35em" id="img-9020955a-270" gadfly:scale="10.0" visibility="hidden">-8.5</text>
    <text x="16.27" y="69.15" text-anchor="end" dy="0.35em" id="img-9020955a-271" gadfly:scale="10.0" visibility="hidden">-8.0</text>
    <text x="16.27" y="66.76" text-anchor="end" dy="0.35em" id="img-9020955a-272" gadfly:scale="10.0" visibility="hidden">-7.5</text>
    <text x="16.27" y="64.37" text-anchor="end" dy="0.35em" id="img-9020955a-273" gadfly:scale="10.0" visibility="hidden">-7.0</text>
    <text x="16.27" y="61.98" text-anchor="end" dy="0.35em" id="img-9020955a-274" gadfly:scale="10.0" visibility="hidden">-6.5</text>
    <text x="16.27" y="59.59" text-anchor="end" dy="0.35em" id="img-9020955a-275" gadfly:scale="10.0" visibility="hidden">-6.0</text>
    <text x="16.27" y="57.2" text-anchor="end" dy="0.35em" id="img-9020955a-276" gadfly:scale="10.0" visibility="hidden">-5.5</text>
    <text x="16.27" y="54.81" text-anchor="end" dy="0.35em" id="img-9020955a-277" gadfly:scale="10.0" visibility="hidden">-5.0</text>
    <text x="16.27" y="52.42" text-anchor="end" dy="0.35em" id="img-9020955a-278" gadfly:scale="10.0" visibility="hidden">-4.5</text>
    <text x="16.27" y="50.03" text-anchor="end" dy="0.35em" id="img-9020955a-279" gadfly:scale="10.0" visibility="hidden">-4.0</text>
    <text x="16.27" y="47.64" text-anchor="end" dy="0.35em" id="img-9020955a-280" gadfly:scale="10.0" visibility="hidden">-3.5</text>
    <text x="16.27" y="45.25" text-anchor="end" dy="0.35em" id="img-9020955a-281" gadfly:scale="10.0" visibility="hidden">-3.0</text>
    <text x="16.27" y="42.86" text-anchor="end" dy="0.35em" id="img-9020955a-282" gadfly:scale="10.0" visibility="hidden">-2.5</text>
    <text x="16.27" y="40.47" text-anchor="end" dy="0.35em" id="img-9020955a-283" gadfly:scale="10.0" visibility="hidden">-2.0</text>
    <text x="16.27" y="38.08" text-anchor="end" dy="0.35em" id="img-9020955a-284" gadfly:scale="10.0" visibility="hidden">-1.5</text>
    <text x="16.27" y="35.69" text-anchor="end" dy="0.35em" id="img-9020955a-285" gadfly:scale="10.0" visibility="hidden">-1.0</text>
    <text x="16.27" y="33.3" text-anchor="end" dy="0.35em" id="img-9020955a-286" gadfly:scale="10.0" visibility="hidden">-0.5</text>
    <text x="16.27" y="30.9" text-anchor="end" dy="0.35em" id="img-9020955a-287" gadfly:scale="10.0" visibility="hidden">0.0</text>
    <text x="16.27" y="28.51" text-anchor="end" dy="0.35em" id="img-9020955a-288" gadfly:scale="10.0" visibility="hidden">0.5</text>
    <text x="16.27" y="26.12" text-anchor="end" dy="0.35em" id="img-9020955a-289" gadfly:scale="10.0" visibility="hidden">1.0</text>
    <text x="16.27" y="23.73" text-anchor="end" dy="0.35em" id="img-9020955a-290" gadfly:scale="10.0" visibility="hidden">1.5</text>
    <text x="16.27" y="21.34" text-anchor="end" dy="0.35em" id="img-9020955a-291" gadfly:scale="10.0" visibility="hidden">2.0</text>
    <text x="16.27" y="18.95" text-anchor="end" dy="0.35em" id="img-9020955a-292" gadfly:scale="10.0" visibility="hidden">2.5</text>
    <text x="16.27" y="16.56" text-anchor="end" dy="0.35em" id="img-9020955a-293" gadfly:scale="10.0" visibility="hidden">3.0</text>
    <text x="16.27" y="14.17" text-anchor="end" dy="0.35em" id="img-9020955a-294" gadfly:scale="10.0" visibility="hidden">3.5</text>
    <text x="16.27" y="11.78" text-anchor="end" dy="0.35em" id="img-9020955a-295" gadfly:scale="10.0" visibility="hidden">4.0</text>
    <text x="16.27" y="9.39" text-anchor="end" dy="0.35em" id="img-9020955a-296" gadfly:scale="10.0" visibility="hidden">4.5</text>
    <text x="16.27" y="7" text-anchor="end" dy="0.35em" id="img-9020955a-297" gadfly:scale="10.0" visibility="hidden">5.0</text>
    <text x="16.27" y="4.61" text-anchor="end" dy="0.35em" id="img-9020955a-298" gadfly:scale="10.0" visibility="hidden">5.5</text>
    <text x="16.27" y="2.22" text-anchor="end" dy="0.35em" id="img-9020955a-299" gadfly:scale="10.0" visibility="hidden">6.0</text>
    <text x="16.27" y="-0.17" text-anchor="end" dy="0.35em" id="img-9020955a-300" gadfly:scale="10.0" visibility="hidden">6.5</text>
    <text x="16.27" y="-2.56" text-anchor="end" dy="0.35em" id="img-9020955a-301" gadfly:scale="10.0" visibility="hidden">7.0</text>
    <text x="16.27" y="-4.95" text-anchor="end" dy="0.35em" id="img-9020955a-302" gadfly:scale="10.0" visibility="hidden">7.5</text>
    <text x="16.27" y="-7.34" text-anchor="end" dy="0.35em" id="img-9020955a-303" gadfly:scale="10.0" visibility="hidden">8.0</text>
    <text x="16.27" y="-9.73" text-anchor="end" dy="0.35em" id="img-9020955a-304" gadfly:scale="10.0" visibility="hidden">8.5</text>
    <text x="16.27" y="-12.12" text-anchor="end" dy="0.35em" id="img-9020955a-305" gadfly:scale="10.0" visibility="hidden">9.0</text>
    <text x="16.27" y="-14.51" text-anchor="end" dy="0.35em" id="img-9020955a-306" gadfly:scale="10.0" visibility="hidden">9.5</text>
    <text x="16.27" y="-16.9" text-anchor="end" dy="0.35em" id="img-9020955a-307" gadfly:scale="10.0" visibility="hidden">10.0</text>
    <text x="16.27" y="-19.3" text-anchor="end" dy="0.35em" id="img-9020955a-308" gadfly:scale="10.0" visibility="hidden">10.5</text>
    <text x="16.27" y="-21.69" text-anchor="end" dy="0.35em" id="img-9020955a-309" gadfly:scale="10.0" visibility="hidden">11.0</text>
    <text x="16.27" y="-24.08" text-anchor="end" dy="0.35em" id="img-9020955a-310" gadfly:scale="10.0" visibility="hidden">11.5</text>
    <text x="16.27" y="-26.47" text-anchor="end" dy="0.35em" id="img-9020955a-311" gadfly:scale="10.0" visibility="hidden">12.0</text>
    <text x="16.27" y="-28.86" text-anchor="end" dy="0.35em" id="img-9020955a-312" gadfly:scale="10.0" visibility="hidden">12.5</text>
    <text x="16.27" y="-31.25" text-anchor="end" dy="0.35em" id="img-9020955a-313" gadfly:scale="10.0" visibility="hidden">13.0</text>
    <text x="16.27" y="-33.64" text-anchor="end" dy="0.35em" id="img-9020955a-314" gadfly:scale="10.0" visibility="hidden">13.5</text>
    <text x="16.27" y="-36.03" text-anchor="end" dy="0.35em" id="img-9020955a-315" gadfly:scale="10.0" visibility="hidden">14.0</text>
    <text x="16.27" y="-38.42" text-anchor="end" dy="0.35em" id="img-9020955a-316" gadfly:scale="10.0" visibility="hidden">14.5</text>
    <text x="16.27" y="-40.81" text-anchor="end" dy="0.35em" id="img-9020955a-317" gadfly:scale="10.0" visibility="hidden">15.0</text>
    <text x="16.27" y="-43.2" text-anchor="end" dy="0.35em" id="img-9020955a-318" gadfly:scale="10.0" visibility="hidden">15.5</text>
    <text x="16.27" y="-45.59" text-anchor="end" dy="0.35em" id="img-9020955a-319" gadfly:scale="10.0" visibility="hidden">16.0</text>
    <text x="16.27" y="-47.98" text-anchor="end" dy="0.35em" id="img-9020955a-320" gadfly:scale="10.0" visibility="hidden">16.5</text>
    <text x="16.27" y="-50.37" text-anchor="end" dy="0.35em" id="img-9020955a-321" gadfly:scale="10.0" visibility="hidden">17.0</text>
    <text x="16.27" y="-52.76" text-anchor="end" dy="0.35em" id="img-9020955a-322" gadfly:scale="10.0" visibility="hidden">17.5</text>
    <text x="16.27" y="-55.15" text-anchor="end" dy="0.35em" id="img-9020955a-323" gadfly:scale="10.0" visibility="hidden">18.0</text>
    <text x="16.27" y="-57.54" text-anchor="end" dy="0.35em" id="img-9020955a-324" gadfly:scale="10.0" visibility="hidden">18.5</text>
    <text x="16.27" y="-59.93" text-anchor="end" dy="0.35em" id="img-9020955a-325" gadfly:scale="10.0" visibility="hidden">19.0</text>
    <text x="16.27" y="-62.32" text-anchor="end" dy="0.35em" id="img-9020955a-326" gadfly:scale="10.0" visibility="hidden">19.5</text>
    <text x="16.27" y="-64.72" text-anchor="end" dy="0.35em" id="img-9020955a-327" gadfly:scale="10.0" visibility="hidden">20.0</text>
    <text x="16.27" y="222.14" text-anchor="end" dy="0.35em" id="img-9020955a-328" gadfly:scale="0.5" visibility="hidden">-40</text>
    <text x="16.27" y="126.52" text-anchor="end" dy="0.35em" id="img-9020955a-329" gadfly:scale="0.5" visibility="hidden">-20</text>
    <text x="16.27" y="30.9" text-anchor="end" dy="0.35em" id="img-9020955a-330" gadfly:scale="0.5" visibility="hidden">0</text>
    <text x="16.27" y="-64.72" text-anchor="end" dy="0.35em" id="img-9020955a-331" gadfly:scale="0.5" visibility="hidden">20</text>
    <text x="16.27" y="150.43" text-anchor="end" dy="0.35em" id="img-9020955a-332" gadfly:scale="5.0" visibility="hidden">-25</text>
    <text x="16.27" y="145.65" text-anchor="end" dy="0.35em" id="img-9020955a-333" gadfly:scale="5.0" visibility="hidden">-24</text>
    <text x="16.27" y="140.87" text-anchor="end" dy="0.35em" id="img-9020955a-334" gadfly:scale="5.0" visibility="hidden">-23</text>
    <text x="16.27" y="136.09" text-anchor="end" dy="0.35em" id="img-9020955a-335" gadfly:scale="5.0" visibility="hidden">-22</text>
    <text x="16.27" y="131.31" text-anchor="end" dy="0.35em" id="img-9020955a-336" gadfly:scale="5.0" visibility="hidden">-21</text>
    <text x="16.27" y="126.52" text-anchor="end" dy="0.35em" id="img-9020955a-337" gadfly:scale="5.0" visibility="hidden">-20</text>
    <text x="16.27" y="121.74" text-anchor="end" dy="0.35em" id="img-9020955a-338" gadfly:scale="5.0" visibility="hidden">-19</text>
    <text x="16.27" y="116.96" text-anchor="end" dy="0.35em" id="img-9020955a-339" gadfly:scale="5.0" visibility="hidden">-18</text>
    <text x="16.27" y="112.18" text-anchor="end" dy="0.35em" id="img-9020955a-340" gadfly:scale="5.0" visibility="hidden">-17</text>
    <text x="16.27" y="107.4" text-anchor="end" dy="0.35em" id="img-9020955a-341" gadfly:scale="5.0" visibility="hidden">-16</text>
    <text x="16.27" y="102.62" text-anchor="end" dy="0.35em" id="img-9020955a-342" gadfly:scale="5.0" visibility="hidden">-15</text>
    <text x="16.27" y="97.84" text-anchor="end" dy="0.35em" id="img-9020955a-343" gadfly:scale="5.0" visibility="hidden">-14</text>
    <text x="16.27" y="93.06" text-anchor="end" dy="0.35em" id="img-9020955a-344" gadfly:scale="5.0" visibility="hidden">-13</text>
    <text x="16.27" y="88.28" text-anchor="end" dy="0.35em" id="img-9020955a-345" gadfly:scale="5.0" visibility="hidden">-12</text>
    <text x="16.27" y="83.5" text-anchor="end" dy="0.35em" id="img-9020955a-346" gadfly:scale="5.0" visibility="hidden">-11</text>
    <text x="16.27" y="78.71" text-anchor="end" dy="0.35em" id="img-9020955a-347" gadfly:scale="5.0" visibility="hidden">-10</text>
    <text x="16.27" y="73.93" text-anchor="end" dy="0.35em" id="img-9020955a-348" gadfly:scale="5.0" visibility="hidden">-9</text>
    <text x="16.27" y="69.15" text-anchor="end" dy="0.35em" id="img-9020955a-349" gadfly:scale="5.0" visibility="hidden">-8</text>
    <text x="16.27" y="64.37" text-anchor="end" dy="0.35em" id="img-9020955a-350" gadfly:scale="5.0" visibility="hidden">-7</text>
    <text x="16.27" y="59.59" text-anchor="end" dy="0.35em" id="img-9020955a-351" gadfly:scale="5.0" visibility="hidden">-6</text>
    <text x="16.27" y="54.81" text-anchor="end" dy="0.35em" id="img-9020955a-352" gadfly:scale="5.0" visibility="hidden">-5</text>
    <text x="16.27" y="50.03" text-anchor="end" dy="0.35em" id="img-9020955a-353" gadfly:scale="5.0" visibility="hidden">-4</text>
    <text x="16.27" y="45.25" text-anchor="end" dy="0.35em" id="img-9020955a-354" gadfly:scale="5.0" visibility="hidden">-3</text>
    <text x="16.27" y="40.47" text-anchor="end" dy="0.35em" id="img-9020955a-355" gadfly:scale="5.0" visibility="hidden">-2</text>
    <text x="16.27" y="35.69" text-anchor="end" dy="0.35em" id="img-9020955a-356" gadfly:scale="5.0" visibility="hidden">-1</text>
    <text x="16.27" y="30.9" text-anchor="end" dy="0.35em" id="img-9020955a-357" gadfly:scale="5.0" visibility="hidden">0</text>
    <text x="16.27" y="26.12" text-anchor="end" dy="0.35em" id="img-9020955a-358" gadfly:scale="5.0" visibility="hidden">1</text>
    <text x="16.27" y="21.34" text-anchor="end" dy="0.35em" id="img-9020955a-359" gadfly:scale="5.0" visibility="hidden">2</text>
    <text x="16.27" y="16.56" text-anchor="end" dy="0.35em" id="img-9020955a-360" gadfly:scale="5.0" visibility="hidden">3</text>
    <text x="16.27" y="11.78" text-anchor="end" dy="0.35em" id="img-9020955a-361" gadfly:scale="5.0" visibility="hidden">4</text>
    <text x="16.27" y="7" text-anchor="end" dy="0.35em" id="img-9020955a-362" gadfly:scale="5.0" visibility="hidden">5</text>
    <text x="16.27" y="2.22" text-anchor="end" dy="0.35em" id="img-9020955a-363" gadfly:scale="5.0" visibility="hidden">6</text>
    <text x="16.27" y="-2.56" text-anchor="end" dy="0.35em" id="img-9020955a-364" gadfly:scale="5.0" visibility="hidden">7</text>
    <text x="16.27" y="-7.34" text-anchor="end" dy="0.35em" id="img-9020955a-365" gadfly:scale="5.0" visibility="hidden">8</text>
    <text x="16.27" y="-12.12" text-anchor="end" dy="0.35em" id="img-9020955a-366" gadfly:scale="5.0" visibility="hidden">9</text>
    <text x="16.27" y="-16.9" text-anchor="end" dy="0.35em" id="img-9020955a-367" gadfly:scale="5.0" visibility="hidden">10</text>
    <text x="16.27" y="-21.69" text-anchor="end" dy="0.35em" id="img-9020955a-368" gadfly:scale="5.0" visibility="hidden">11</text>
    <text x="16.27" y="-26.47" text-anchor="end" dy="0.35em" id="img-9020955a-369" gadfly:scale="5.0" visibility="hidden">12</text>
    <text x="16.27" y="-31.25" text-anchor="end" dy="0.35em" id="img-9020955a-370" gadfly:scale="5.0" visibility="hidden">13</text>
    <text x="16.27" y="-36.03" text-anchor="end" dy="0.35em" id="img-9020955a-371" gadfly:scale="5.0" visibility="hidden">14</text>
    <text x="16.27" y="-40.81" text-anchor="end" dy="0.35em" id="img-9020955a-372" gadfly:scale="5.0" visibility="hidden">15</text>
    <text x="16.27" y="-45.59" text-anchor="end" dy="0.35em" id="img-9020955a-373" gadfly:scale="5.0" visibility="hidden">16</text>
    <text x="16.27" y="-50.37" text-anchor="end" dy="0.35em" id="img-9020955a-374" gadfly:scale="5.0" visibility="hidden">17</text>
    <text x="16.27" y="-55.15" text-anchor="end" dy="0.35em" id="img-9020955a-375" gadfly:scale="5.0" visibility="hidden">18</text>
    <text x="16.27" y="-59.93" text-anchor="end" dy="0.35em" id="img-9020955a-376" gadfly:scale="5.0" visibility="hidden">19</text>
    <text x="16.27" y="-64.72" text-anchor="end" dy="0.35em" id="img-9020955a-377" gadfly:scale="5.0" visibility="hidden">20</text>
  </g>
  <g font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" fill="#564A55" stroke="#000000" stroke-opacity="0.000" id="img-9020955a-378">
    <text x="8.81" y="42.86" text-anchor="end" dy="0.35em" id="img-9020955a-379">y</text>
  </g>
</g>
<defs>
  <clipPath id="img-9020955a-15">
  <path d="M17.27,5 L 117.45 5 117.45 80.72 17.27 80.72" />
</clipPath>
</defs>
<script> <![CDATA[
(function(N){var k=/[\.\/]/,L=/\s*,\s*/,C=function(a,d){return a-d},a,v,y={n:{}},M=function(){for(var a=0,d=this.length;a<d;a++)if("undefined"!=typeof this[a])return this[a]},A=function(){for(var a=this.length;--a;)if("undefined"!=typeof this[a])return this[a]},w=function(k,d){k=String(k);var f=v,n=Array.prototype.slice.call(arguments,2),u=w.listeners(k),p=0,b,q=[],e={},l=[],r=a;l.firstDefined=M;l.lastDefined=A;a=k;for(var s=v=0,x=u.length;s<x;s++)"zIndex"in u[s]&&(q.push(u[s].zIndex),0>u[s].zIndex&&
(e[u[s].zIndex]=u[s]));for(q.sort(C);0>q[p];)if(b=e[q[p++] ],l.push(b.apply(d,n)),v)return v=f,l;for(s=0;s<x;s++)if(b=u[s],"zIndex"in b)if(b.zIndex==q[p]){l.push(b.apply(d,n));if(v)break;do if(p++,(b=e[q[p] ])&&l.push(b.apply(d,n)),v)break;while(b)}else e[b.zIndex]=b;else if(l.push(b.apply(d,n)),v)break;v=f;a=r;return l};w._events=y;w.listeners=function(a){a=a.split(k);var d=y,f,n,u,p,b,q,e,l=[d],r=[];u=0;for(p=a.length;u<p;u++){e=[];b=0;for(q=l.length;b<q;b++)for(d=l[b].n,f=[d[a[u] ],d["*"] ],n=2;n--;)if(d=
f[n])e.push(d),r=r.concat(d.f||[]);l=e}return r};w.on=function(a,d){a=String(a);if("function"!=typeof d)return function(){};for(var f=a.split(L),n=0,u=f.length;n<u;n++)(function(a){a=a.split(k);for(var b=y,f,e=0,l=a.length;e<l;e++)b=b.n,b=b.hasOwnProperty(a[e])&&b[a[e] ]||(b[a[e] ]={n:{}});b.f=b.f||[];e=0;for(l=b.f.length;e<l;e++)if(b.f[e]==d){f=!0;break}!f&&b.f.push(d)})(f[n]);return function(a){+a==+a&&(d.zIndex=+a)}};w.f=function(a){var d=[].slice.call(arguments,1);return function(){w.apply(null,
[a,null].concat(d).concat([].slice.call(arguments,0)))}};w.stop=function(){v=1};w.nt=function(k){return k?(new RegExp("(?:\\.|\\/|^)"+k+"(?:\\.|\\/|$)")).test(a):a};w.nts=function(){return a.split(k)};w.off=w.unbind=function(a,d){if(a){var f=a.split(L);if(1<f.length)for(var n=0,u=f.length;n<u;n++)w.off(f[n],d);else{for(var f=a.split(k),p,b,q,e,l=[y],n=0,u=f.length;n<u;n++)for(e=0;e<l.length;e+=q.length-2){q=[e,1];p=l[e].n;if("*"!=f[n])p[f[n] ]&&q.push(p[f[n] ]);else for(b in p)p.hasOwnProperty(b)&&
q.push(p[b]);l.splice.apply(l,q)}n=0;for(u=l.length;n<u;n++)for(p=l[n];p.n;){if(d){if(p.f){e=0;for(f=p.f.length;e<f;e++)if(p.f[e]==d){p.f.splice(e,1);break}!p.f.length&&delete p.f}for(b in p.n)if(p.n.hasOwnProperty(b)&&p.n[b].f){q=p.n[b].f;e=0;for(f=q.length;e<f;e++)if(q[e]==d){q.splice(e,1);break}!q.length&&delete p.n[b].f}}else for(b in delete p.f,p.n)p.n.hasOwnProperty(b)&&p.n[b].f&&delete p.n[b].f;p=p.n}}}else w._events=y={n:{}}};w.once=function(a,d){var f=function(){w.unbind(a,f);return d.apply(this,
arguments)};return w.on(a,f)};w.version="0.4.2";w.toString=function(){return"You are running Eve 0.4.2"};"undefined"!=typeof module&&module.exports?module.exports=w:"function"===typeof define&&define.amd?define("eve",[],function(){return w}):N.eve=w})(this);
(function(N,k){"function"===typeof define&&define.amd?define("Snap.svg",["eve"],function(L){return k(N,L)}):k(N,N.eve)})(this,function(N,k){var L=function(a){var k={},y=N.requestAnimationFrame||N.webkitRequestAnimationFrame||N.mozRequestAnimationFrame||N.oRequestAnimationFrame||N.msRequestAnimationFrame||function(a){setTimeout(a,16)},M=Array.isArray||function(a){return a instanceof Array||"[object Array]"==Object.prototype.toString.call(a)},A=0,w="M"+(+new Date).toString(36),z=function(a){if(null==
a)return this.s;var b=this.s-a;this.b+=this.dur*b;this.B+=this.dur*b;this.s=a},d=function(a){if(null==a)return this.spd;this.spd=a},f=function(a){if(null==a)return this.dur;this.s=this.s*a/this.dur;this.dur=a},n=function(){delete k[this.id];this.update();a("mina.stop."+this.id,this)},u=function(){this.pdif||(delete k[this.id],this.update(),this.pdif=this.get()-this.b)},p=function(){this.pdif&&(this.b=this.get()-this.pdif,delete this.pdif,k[this.id]=this)},b=function(){var a;if(M(this.start)){a=[];
for(var b=0,e=this.start.length;b<e;b++)a[b]=+this.start[b]+(this.end[b]-this.start[b])*this.easing(this.s)}else a=+this.start+(this.end-this.start)*this.easing(this.s);this.set(a)},q=function(){var l=0,b;for(b in k)if(k.hasOwnProperty(b)){var e=k[b],f=e.get();l++;e.s=(f-e.b)/(e.dur/e.spd);1<=e.s&&(delete k[b],e.s=1,l--,function(b){setTimeout(function(){a("mina.finish."+b.id,b)})}(e));e.update()}l&&y(q)},e=function(a,r,s,x,G,h,J){a={id:w+(A++).toString(36),start:a,end:r,b:s,s:0,dur:x-s,spd:1,get:G,
set:h,easing:J||e.linear,status:z,speed:d,duration:f,stop:n,pause:u,resume:p,update:b};k[a.id]=a;r=0;for(var K in k)if(k.hasOwnProperty(K)&&(r++,2==r))break;1==r&&y(q);return a};e.time=Date.now||function(){return+new Date};e.getById=function(a){return k[a]||null};e.linear=function(a){return a};e.easeout=function(a){return Math.pow(a,1.7)};e.easein=function(a){return Math.pow(a,0.48)};e.easeinout=function(a){if(1==a)return 1;if(0==a)return 0;var b=0.48-a/1.04,e=Math.sqrt(0.1734+b*b);a=e-b;a=Math.pow(Math.abs(a),
1/3)*(0>a?-1:1);b=-e-b;b=Math.pow(Math.abs(b),1/3)*(0>b?-1:1);a=a+b+0.5;return 3*(1-a)*a*a+a*a*a};e.backin=function(a){return 1==a?1:a*a*(2.70158*a-1.70158)};e.backout=function(a){if(0==a)return 0;a-=1;return a*a*(2.70158*a+1.70158)+1};e.elastic=function(a){return a==!!a?a:Math.pow(2,-10*a)*Math.sin(2*(a-0.075)*Math.PI/0.3)+1};e.bounce=function(a){a<1/2.75?a*=7.5625*a:a<2/2.75?(a-=1.5/2.75,a=7.5625*a*a+0.75):a<2.5/2.75?(a-=2.25/2.75,a=7.5625*a*a+0.9375):(a-=2.625/2.75,a=7.5625*a*a+0.984375);return a};
return N.mina=e}("undefined"==typeof k?function(){}:k),C=function(){function a(c,t){if(c){if(c.tagName)return x(c);if(y(c,"array")&&a.set)return a.set.apply(a,c);if(c instanceof e)return c;if(null==t)return c=G.doc.querySelector(c),x(c)}return new s(null==c?"100%":c,null==t?"100%":t)}function v(c,a){if(a){"#text"==c&&(c=G.doc.createTextNode(a.text||""));"string"==typeof c&&(c=v(c));if("string"==typeof a)return"xlink:"==a.substring(0,6)?c.getAttributeNS(m,a.substring(6)):"xml:"==a.substring(0,4)?c.getAttributeNS(la,
a.substring(4)):c.getAttribute(a);for(var da in a)if(a[h](da)){var b=J(a[da]);b?"xlink:"==da.substring(0,6)?c.setAttributeNS(m,da.substring(6),b):"xml:"==da.substring(0,4)?c.setAttributeNS(la,da.substring(4),b):c.setAttribute(da,b):c.removeAttribute(da)}}else c=G.doc.createElementNS(la,c);return c}function y(c,a){a=J.prototype.toLowerCase.call(a);return"finite"==a?isFinite(c):"array"==a&&(c instanceof Array||Array.isArray&&Array.isArray(c))?!0:"null"==a&&null===c||a==typeof c&&null!==c||"object"==
a&&c===Object(c)||$.call(c).slice(8,-1).toLowerCase()==a}function M(c){if("function"==typeof c||Object(c)!==c)return c;var a=new c.constructor,b;for(b in c)c[h](b)&&(a[b]=M(c[b]));return a}function A(c,a,b){function m(){var e=Array.prototype.slice.call(arguments,0),f=e.join("\u2400"),d=m.cache=m.cache||{},l=m.count=m.count||[];if(d[h](f)){a:for(var e=l,l=f,B=0,H=e.length;B<H;B++)if(e[B]===l){e.push(e.splice(B,1)[0]);break a}return b?b(d[f]):d[f]}1E3<=l.length&&delete d[l.shift()];l.push(f);d[f]=c.apply(a,
e);return b?b(d[f]):d[f]}return m}function w(c,a,b,m,e,f){return null==e?(c-=b,a-=m,c||a?(180*I.atan2(-a,-c)/C+540)%360:0):w(c,a,e,f)-w(b,m,e,f)}function z(c){return c%360*C/180}function d(c){var a=[];c=c.replace(/(?:^|\s)(\w+)\(([^)]+)\)/g,function(c,b,m){m=m.split(/\s*,\s*|\s+/);"rotate"==b&&1==m.length&&m.push(0,0);"scale"==b&&(2<m.length?m=m.slice(0,2):2==m.length&&m.push(0,0),1==m.length&&m.push(m[0],0,0));"skewX"==b?a.push(["m",1,0,I.tan(z(m[0])),1,0,0]):"skewY"==b?a.push(["m",1,I.tan(z(m[0])),
0,1,0,0]):a.push([b.charAt(0)].concat(m));return c});return a}function f(c,t){var b=O(c),m=new a.Matrix;if(b)for(var e=0,f=b.length;e<f;e++){var h=b[e],d=h.length,B=J(h[0]).toLowerCase(),H=h[0]!=B,l=H?m.invert():0,E;"t"==B&&2==d?m.translate(h[1],0):"t"==B&&3==d?H?(d=l.x(0,0),B=l.y(0,0),H=l.x(h[1],h[2]),l=l.y(h[1],h[2]),m.translate(H-d,l-B)):m.translate(h[1],h[2]):"r"==B?2==d?(E=E||t,m.rotate(h[1],E.x+E.width/2,E.y+E.height/2)):4==d&&(H?(H=l.x(h[2],h[3]),l=l.y(h[2],h[3]),m.rotate(h[1],H,l)):m.rotate(h[1],
h[2],h[3])):"s"==B?2==d||3==d?(E=E||t,m.scale(h[1],h[d-1],E.x+E.width/2,E.y+E.height/2)):4==d?H?(H=l.x(h[2],h[3]),l=l.y(h[2],h[3]),m.scale(h[1],h[1],H,l)):m.scale(h[1],h[1],h[2],h[3]):5==d&&(H?(H=l.x(h[3],h[4]),l=l.y(h[3],h[4]),m.scale(h[1],h[2],H,l)):m.scale(h[1],h[2],h[3],h[4])):"m"==B&&7==d&&m.add(h[1],h[2],h[3],h[4],h[5],h[6])}return m}function n(c,t){if(null==t){var m=!0;t="linearGradient"==c.type||"radialGradient"==c.type?c.node.getAttribute("gradientTransform"):"pattern"==c.type?c.node.getAttribute("patternTransform"):
c.node.getAttribute("transform");if(!t)return new a.Matrix;t=d(t)}else t=a._.rgTransform.test(t)?J(t).replace(/\.{3}|\u2026/g,c._.transform||aa):d(t),y(t,"array")&&(t=a.path?a.path.toString.call(t):J(t)),c._.transform=t;var b=f(t,c.getBBox(1));if(m)return b;c.matrix=b}function u(c){c=c.node.ownerSVGElement&&x(c.node.ownerSVGElement)||c.node.parentNode&&x(c.node.parentNode)||a.select("svg")||a(0,0);var t=c.select("defs"),t=null==t?!1:t.node;t||(t=r("defs",c.node).node);return t}function p(c){return c.node.ownerSVGElement&&
x(c.node.ownerSVGElement)||a.select("svg")}function b(c,a,m){function b(c){if(null==c)return aa;if(c==+c)return c;v(B,{width:c});try{return B.getBBox().width}catch(a){return 0}}function h(c){if(null==c)return aa;if(c==+c)return c;v(B,{height:c});try{return B.getBBox().height}catch(a){return 0}}function e(b,B){null==a?d[b]=B(c.attr(b)||0):b==a&&(d=B(null==m?c.attr(b)||0:m))}var f=p(c).node,d={},B=f.querySelector(".svg---mgr");B||(B=v("rect"),v(B,{x:-9E9,y:-9E9,width:10,height:10,"class":"svg---mgr",
fill:"none"}),f.appendChild(B));switch(c.type){case "rect":e("rx",b),e("ry",h);case "image":e("width",b),e("height",h);case "text":e("x",b);e("y",h);break;case "circle":e("cx",b);e("cy",h);e("r",b);break;case "ellipse":e("cx",b);e("cy",h);e("rx",b);e("ry",h);break;case "line":e("x1",b);e("x2",b);e("y1",h);e("y2",h);break;case "marker":e("refX",b);e("markerWidth",b);e("refY",h);e("markerHeight",h);break;case "radialGradient":e("fx",b);e("fy",h);break;case "tspan":e("dx",b);e("dy",h);break;default:e(a,
b)}f.removeChild(B);return d}function q(c){y(c,"array")||(c=Array.prototype.slice.call(arguments,0));for(var a=0,b=0,m=this.node;this[a];)delete this[a++];for(a=0;a<c.length;a++)"set"==c[a].type?c[a].forEach(function(c){m.appendChild(c.node)}):m.appendChild(c[a].node);for(var h=m.childNodes,a=0;a<h.length;a++)this[b++]=x(h[a]);return this}function e(c){if(c.snap in E)return E[c.snap];var a=this.id=V(),b;try{b=c.ownerSVGElement}catch(m){}this.node=c;b&&(this.paper=new s(b));this.type=c.tagName;this.anims=
{};this._={transform:[]};c.snap=a;E[a]=this;"g"==this.type&&(this.add=q);if(this.type in{g:1,mask:1,pattern:1})for(var e in s.prototype)s.prototype[h](e)&&(this[e]=s.prototype[e])}function l(c){this.node=c}function r(c,a){var b=v(c);a.appendChild(b);return x(b)}function s(c,a){var b,m,f,d=s.prototype;if(c&&"svg"==c.tagName){if(c.snap in E)return E[c.snap];var l=c.ownerDocument;b=new e(c);m=c.getElementsByTagName("desc")[0];f=c.getElementsByTagName("defs")[0];m||(m=v("desc"),m.appendChild(l.createTextNode("Created with Snap")),
b.node.appendChild(m));f||(f=v("defs"),b.node.appendChild(f));b.defs=f;for(var ca in d)d[h](ca)&&(b[ca]=d[ca]);b.paper=b.root=b}else b=r("svg",G.doc.body),v(b.node,{height:a,version:1.1,width:c,xmlns:la});return b}function x(c){return!c||c instanceof e||c instanceof l?c:c.tagName&&"svg"==c.tagName.toLowerCase()?new s(c):c.tagName&&"object"==c.tagName.toLowerCase()&&"image/svg+xml"==c.type?new s(c.contentDocument.getElementsByTagName("svg")[0]):new e(c)}a.version="0.3.0";a.toString=function(){return"Snap v"+
this.version};a._={};var G={win:N,doc:N.document};a._.glob=G;var h="hasOwnProperty",J=String,K=parseFloat,U=parseInt,I=Math,P=I.max,Q=I.min,Y=I.abs,C=I.PI,aa="",$=Object.prototype.toString,F=/^\s*((#[a-f\d]{6})|(#[a-f\d]{3})|rgba?\(\s*([\d\.]+%?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+%?(?:\s*,\s*[\d\.]+%?)?)\s*\)|hsba?\(\s*([\d\.]+(?:deg|\xb0|%)?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+(?:%?\s*,\s*[\d\.]+)?%?)\s*\)|hsla?\(\s*([\d\.]+(?:deg|\xb0|%)?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+(?:%?\s*,\s*[\d\.]+)?%?)\s*\))\s*$/i;a._.separator=
RegExp("[,\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]+");var S=RegExp("[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*"),X={hs:1,rg:1},W=RegExp("([a-z])[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029,]*((-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*)+)",
"ig"),ma=RegExp("([rstm])[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029,]*((-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*)+)","ig"),Z=RegExp("(-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?)[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*",
"ig"),na=0,ba="S"+(+new Date).toString(36),V=function(){return ba+(na++).toString(36)},m="http://www.w3.org/1999/xlink",la="http://www.w3.org/2000/svg",E={},ca=a.url=function(c){return"url('#"+c+"')"};a._.$=v;a._.id=V;a.format=function(){var c=/\{([^\}]+)\}/g,a=/(?:(?:^|\.)(.+?)(?=\[|\.|$|\()|\[('|")(.+?)\2\])(\(\))?/g,b=function(c,b,m){var h=m;b.replace(a,function(c,a,b,m,t){a=a||m;h&&(a in h&&(h=h[a]),"function"==typeof h&&t&&(h=h()))});return h=(null==h||h==m?c:h)+""};return function(a,m){return J(a).replace(c,
function(c,a){return b(c,a,m)})}}();a._.clone=M;a._.cacher=A;a.rad=z;a.deg=function(c){return 180*c/C%360};a.angle=w;a.is=y;a.snapTo=function(c,a,b){b=y(b,"finite")?b:10;if(y(c,"array"))for(var m=c.length;m--;){if(Y(c[m]-a)<=b)return c[m]}else{c=+c;m=a%c;if(m<b)return a-m;if(m>c-b)return a-m+c}return a};a.getRGB=A(function(c){if(!c||(c=J(c)).indexOf("-")+1)return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka};if("none"==c)return{r:-1,g:-1,b:-1,hex:"none",toString:ka};!X[h](c.toLowerCase().substring(0,
2))&&"#"!=c.charAt()&&(c=T(c));if(!c)return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka};var b,m,e,f,d;if(c=c.match(F)){c[2]&&(e=U(c[2].substring(5),16),m=U(c[2].substring(3,5),16),b=U(c[2].substring(1,3),16));c[3]&&(e=U((d=c[3].charAt(3))+d,16),m=U((d=c[3].charAt(2))+d,16),b=U((d=c[3].charAt(1))+d,16));c[4]&&(d=c[4].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b*=2.55),m=K(d[1]),"%"==d[1].slice(-1)&&(m*=2.55),e=K(d[2]),"%"==d[2].slice(-1)&&(e*=2.55),"rgba"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),
d[3]&&"%"==d[3].slice(-1)&&(f/=100));if(c[5])return d=c[5].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b/=100),m=K(d[1]),"%"==d[1].slice(-1)&&(m/=100),e=K(d[2]),"%"==d[2].slice(-1)&&(e/=100),"deg"!=d[0].slice(-3)&&"\u00b0"!=d[0].slice(-1)||(b/=360),"hsba"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),d[3]&&"%"==d[3].slice(-1)&&(f/=100),a.hsb2rgb(b,m,e,f);if(c[6])return d=c[6].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b/=100),m=K(d[1]),"%"==d[1].slice(-1)&&(m/=100),e=K(d[2]),"%"==d[2].slice(-1)&&(e/=100),
"deg"!=d[0].slice(-3)&&"\u00b0"!=d[0].slice(-1)||(b/=360),"hsla"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),d[3]&&"%"==d[3].slice(-1)&&(f/=100),a.hsl2rgb(b,m,e,f);b=Q(I.round(b),255);m=Q(I.round(m),255);e=Q(I.round(e),255);f=Q(P(f,0),1);c={r:b,g:m,b:e,toString:ka};c.hex="#"+(16777216|e|m<<8|b<<16).toString(16).slice(1);c.opacity=y(f,"finite")?f:1;return c}return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka}},a);a.hsb=A(function(c,b,m){return a.hsb2rgb(c,b,m).hex});a.hsl=A(function(c,b,m){return a.hsl2rgb(c,
b,m).hex});a.rgb=A(function(c,a,b,m){if(y(m,"finite")){var e=I.round;return"rgba("+[e(c),e(a),e(b),+m.toFixed(2)]+")"}return"#"+(16777216|b|a<<8|c<<16).toString(16).slice(1)});var T=function(c){var a=G.doc.getElementsByTagName("head")[0]||G.doc.getElementsByTagName("svg")[0];T=A(function(c){if("red"==c.toLowerCase())return"rgb(255, 0, 0)";a.style.color="rgb(255, 0, 0)";a.style.color=c;c=G.doc.defaultView.getComputedStyle(a,aa).getPropertyValue("color");return"rgb(255, 0, 0)"==c?null:c});return T(c)},
qa=function(){return"hsb("+[this.h,this.s,this.b]+")"},ra=function(){return"hsl("+[this.h,this.s,this.l]+")"},ka=function(){return 1==this.opacity||null==this.opacity?this.hex:"rgba("+[this.r,this.g,this.b,this.opacity]+")"},D=function(c,b,m){null==b&&y(c,"object")&&"r"in c&&"g"in c&&"b"in c&&(m=c.b,b=c.g,c=c.r);null==b&&y(c,string)&&(m=a.getRGB(c),c=m.r,b=m.g,m=m.b);if(1<c||1<b||1<m)c/=255,b/=255,m/=255;return[c,b,m]},oa=function(c,b,m,e){c=I.round(255*c);b=I.round(255*b);m=I.round(255*m);c={r:c,
g:b,b:m,opacity:y(e,"finite")?e:1,hex:a.rgb(c,b,m),toString:ka};y(e,"finite")&&(c.opacity=e);return c};a.color=function(c){var b;y(c,"object")&&"h"in c&&"s"in c&&"b"in c?(b=a.hsb2rgb(c),c.r=b.r,c.g=b.g,c.b=b.b,c.opacity=1,c.hex=b.hex):y(c,"object")&&"h"in c&&"s"in c&&"l"in c?(b=a.hsl2rgb(c),c.r=b.r,c.g=b.g,c.b=b.b,c.opacity=1,c.hex=b.hex):(y(c,"string")&&(c=a.getRGB(c)),y(c,"object")&&"r"in c&&"g"in c&&"b"in c&&!("error"in c)?(b=a.rgb2hsl(c),c.h=b.h,c.s=b.s,c.l=b.l,b=a.rgb2hsb(c),c.v=b.b):(c={hex:"none"},
c.r=c.g=c.b=c.h=c.s=c.v=c.l=-1,c.error=1));c.toString=ka;return c};a.hsb2rgb=function(c,a,b,m){y(c,"object")&&"h"in c&&"s"in c&&"b"in c&&(b=c.b,a=c.s,c=c.h,m=c.o);var e,h,d;c=360*c%360/60;d=b*a;a=d*(1-Y(c%2-1));b=e=h=b-d;c=~~c;b+=[d,a,0,0,a,d][c];e+=[a,d,d,a,0,0][c];h+=[0,0,a,d,d,a][c];return oa(b,e,h,m)};a.hsl2rgb=function(c,a,b,m){y(c,"object")&&"h"in c&&"s"in c&&"l"in c&&(b=c.l,a=c.s,c=c.h);if(1<c||1<a||1<b)c/=360,a/=100,b/=100;var e,h,d;c=360*c%360/60;d=2*a*(0.5>b?b:1-b);a=d*(1-Y(c%2-1));b=e=
h=b-d/2;c=~~c;b+=[d,a,0,0,a,d][c];e+=[a,d,d,a,0,0][c];h+=[0,0,a,d,d,a][c];return oa(b,e,h,m)};a.rgb2hsb=function(c,a,b){b=D(c,a,b);c=b[0];a=b[1];b=b[2];var m,e;m=P(c,a,b);e=m-Q(c,a,b);c=((0==e?0:m==c?(a-b)/e:m==a?(b-c)/e+2:(c-a)/e+4)+360)%6*60/360;return{h:c,s:0==e?0:e/m,b:m,toString:qa}};a.rgb2hsl=function(c,a,b){b=D(c,a,b);c=b[0];a=b[1];b=b[2];var m,e,h;m=P(c,a,b);e=Q(c,a,b);h=m-e;c=((0==h?0:m==c?(a-b)/h:m==a?(b-c)/h+2:(c-a)/h+4)+360)%6*60/360;m=(m+e)/2;return{h:c,s:0==h?0:0.5>m?h/(2*m):h/(2-2*
m),l:m,toString:ra}};a.parsePathString=function(c){if(!c)return null;var b=a.path(c);if(b.arr)return a.path.clone(b.arr);var m={a:7,c:6,o:2,h:1,l:2,m:2,r:4,q:4,s:4,t:2,v:1,u:3,z:0},e=[];y(c,"array")&&y(c[0],"array")&&(e=a.path.clone(c));e.length||J(c).replace(W,function(c,a,b){var h=[];c=a.toLowerCase();b.replace(Z,function(c,a){a&&h.push(+a)});"m"==c&&2<h.length&&(e.push([a].concat(h.splice(0,2))),c="l",a="m"==a?"l":"L");"o"==c&&1==h.length&&e.push([a,h[0] ]);if("r"==c)e.push([a].concat(h));else for(;h.length>=
m[c]&&(e.push([a].concat(h.splice(0,m[c]))),m[c]););});e.toString=a.path.toString;b.arr=a.path.clone(e);return e};var O=a.parseTransformString=function(c){if(!c)return null;var b=[];y(c,"array")&&y(c[0],"array")&&(b=a.path.clone(c));b.length||J(c).replace(ma,function(c,a,m){var e=[];a.toLowerCase();m.replace(Z,function(c,a){a&&e.push(+a)});b.push([a].concat(e))});b.toString=a.path.toString;return b};a._.svgTransform2string=d;a._.rgTransform=RegExp("^[a-z][\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*-?\\.?\\d",
"i");a._.transform2matrix=f;a._unit2px=b;a._.getSomeDefs=u;a._.getSomeSVG=p;a.select=function(c){return x(G.doc.querySelector(c))};a.selectAll=function(c){c=G.doc.querySelectorAll(c);for(var b=(a.set||Array)(),m=0;m<c.length;m++)b.push(x(c[m]));return b};setInterval(function(){for(var c in E)if(E[h](c)){var a=E[c],b=a.node;("svg"!=a.type&&!b.ownerSVGElement||"svg"==a.type&&(!b.parentNode||"ownerSVGElement"in b.parentNode&&!b.ownerSVGElement))&&delete E[c]}},1E4);(function(c){function m(c){function a(c,
b){var m=v(c.node,b);(m=(m=m&&m.match(d))&&m[2])&&"#"==m.charAt()&&(m=m.substring(1))&&(f[m]=(f[m]||[]).concat(function(a){var m={};m[b]=ca(a);v(c.node,m)}))}function b(c){var a=v(c.node,"xlink:href");a&&"#"==a.charAt()&&(a=a.substring(1))&&(f[a]=(f[a]||[]).concat(function(a){c.attr("xlink:href","#"+a)}))}var e=c.selectAll("*"),h,d=/^\s*url\(("|'|)(.*)\1\)\s*$/;c=[];for(var f={},l=0,E=e.length;l<E;l++){h=e[l];a(h,"fill");a(h,"stroke");a(h,"filter");a(h,"mask");a(h,"clip-path");b(h);var t=v(h.node,
"id");t&&(v(h.node,{id:h.id}),c.push({old:t,id:h.id}))}l=0;for(E=c.length;l<E;l++)if(e=f[c[l].old])for(h=0,t=e.length;h<t;h++)e[h](c[l].id)}function e(c,a,b){return function(m){m=m.slice(c,a);1==m.length&&(m=m[0]);return b?b(m):m}}function d(c){return function(){var a=c?"<"+this.type:"",b=this.node.attributes,m=this.node.childNodes;if(c)for(var e=0,h=b.length;e<h;e++)a+=" "+b[e].name+'="'+b[e].value.replace(/"/g,'\\"')+'"';if(m.length){c&&(a+=">");e=0;for(h=m.length;e<h;e++)3==m[e].nodeType?a+=m[e].nodeValue:
1==m[e].nodeType&&(a+=x(m[e]).toString());c&&(a+="</"+this.type+">")}else c&&(a+="/>");return a}}c.attr=function(c,a){if(!c)return this;if(y(c,"string"))if(1<arguments.length){var b={};b[c]=a;c=b}else return k("snap.util.getattr."+c,this).firstDefined();for(var m in c)c[h](m)&&k("snap.util.attr."+m,this,c[m]);return this};c.getBBox=function(c){if(!a.Matrix||!a.path)return this.node.getBBox();var b=this,m=new a.Matrix;if(b.removed)return a._.box();for(;"use"==b.type;)if(c||(m=m.add(b.transform().localMatrix.translate(b.attr("x")||
0,b.attr("y")||0))),b.original)b=b.original;else var e=b.attr("xlink:href"),b=b.original=b.node.ownerDocument.getElementById(e.substring(e.indexOf("#")+1));var e=b._,h=a.path.get[b.type]||a.path.get.deflt;try{if(c)return e.bboxwt=h?a.path.getBBox(b.realPath=h(b)):a._.box(b.node.getBBox()),a._.box(e.bboxwt);b.realPath=h(b);b.matrix=b.transform().localMatrix;e.bbox=a.path.getBBox(a.path.map(b.realPath,m.add(b.matrix)));return a._.box(e.bbox)}catch(d){return a._.box()}};var f=function(){return this.string};
c.transform=function(c){var b=this._;if(null==c){var m=this;c=new a.Matrix(this.node.getCTM());for(var e=n(this),h=[e],d=new a.Matrix,l=e.toTransformString(),b=J(e)==J(this.matrix)?J(b.transform):l;"svg"!=m.type&&(m=m.parent());)h.push(n(m));for(m=h.length;m--;)d.add(h[m]);return{string:b,globalMatrix:c,totalMatrix:d,localMatrix:e,diffMatrix:c.clone().add(e.invert()),global:c.toTransformString(),total:d.toTransformString(),local:l,toString:f}}c instanceof a.Matrix?this.matrix=c:n(this,c);this.node&&
("linearGradient"==this.type||"radialGradient"==this.type?v(this.node,{gradientTransform:this.matrix}):"pattern"==this.type?v(this.node,{patternTransform:this.matrix}):v(this.node,{transform:this.matrix}));return this};c.parent=function(){return x(this.node.parentNode)};c.append=c.add=function(c){if(c){if("set"==c.type){var a=this;c.forEach(function(c){a.add(c)});return this}c=x(c);this.node.appendChild(c.node);c.paper=this.paper}return this};c.appendTo=function(c){c&&(c=x(c),c.append(this));return this};
c.prepend=function(c){if(c){if("set"==c.type){var a=this,b;c.forEach(function(c){b?b.after(c):a.prepend(c);b=c});return this}c=x(c);var m=c.parent();this.node.insertBefore(c.node,this.node.firstChild);this.add&&this.add();c.paper=this.paper;this.parent()&&this.parent().add();m&&m.add()}return this};c.prependTo=function(c){c=x(c);c.prepend(this);return this};c.before=function(c){if("set"==c.type){var a=this;c.forEach(function(c){var b=c.parent();a.node.parentNode.insertBefore(c.node,a.node);b&&b.add()});
this.parent().add();return this}c=x(c);var b=c.parent();this.node.parentNode.insertBefore(c.node,this.node);this.parent()&&this.parent().add();b&&b.add();c.paper=this.paper;return this};c.after=function(c){c=x(c);var a=c.parent();this.node.nextSibling?this.node.parentNode.insertBefore(c.node,this.node.nextSibling):this.node.parentNode.appendChild(c.node);this.parent()&&this.parent().add();a&&a.add();c.paper=this.paper;return this};c.insertBefore=function(c){c=x(c);var a=this.parent();c.node.parentNode.insertBefore(this.node,
c.node);this.paper=c.paper;a&&a.add();c.parent()&&c.parent().add();return this};c.insertAfter=function(c){c=x(c);var a=this.parent();c.node.parentNode.insertBefore(this.node,c.node.nextSibling);this.paper=c.paper;a&&a.add();c.parent()&&c.parent().add();return this};c.remove=function(){var c=this.parent();this.node.parentNode&&this.node.parentNode.removeChild(this.node);delete this.paper;this.removed=!0;c&&c.add();return this};c.select=function(c){return x(this.node.querySelector(c))};c.selectAll=
function(c){c=this.node.querySelectorAll(c);for(var b=(a.set||Array)(),m=0;m<c.length;m++)b.push(x(c[m]));return b};c.asPX=function(c,a){null==a&&(a=this.attr(c));return+b(this,c,a)};c.use=function(){var c,a=this.node.id;a||(a=this.id,v(this.node,{id:a}));c="linearGradient"==this.type||"radialGradient"==this.type||"pattern"==this.type?r(this.type,this.node.parentNode):r("use",this.node.parentNode);v(c.node,{"xlink:href":"#"+a});c.original=this;return c};var l=/\S+/g;c.addClass=function(c){var a=(c||
"").match(l)||[];c=this.node;var b=c.className.baseVal,m=b.match(l)||[],e,h,d;if(a.length){for(e=0;d=a[e++];)h=m.indexOf(d),~h||m.push(d);a=m.join(" ");b!=a&&(c.className.baseVal=a)}return this};c.removeClass=function(c){var a=(c||"").match(l)||[];c=this.node;var b=c.className.baseVal,m=b.match(l)||[],e,h;if(m.length){for(e=0;h=a[e++];)h=m.indexOf(h),~h&&m.splice(h,1);a=m.join(" ");b!=a&&(c.className.baseVal=a)}return this};c.hasClass=function(c){return!!~(this.node.className.baseVal.match(l)||[]).indexOf(c)};
c.toggleClass=function(c,a){if(null!=a)return a?this.addClass(c):this.removeClass(c);var b=(c||"").match(l)||[],m=this.node,e=m.className.baseVal,h=e.match(l)||[],d,f,E;for(d=0;E=b[d++];)f=h.indexOf(E),~f?h.splice(f,1):h.push(E);b=h.join(" ");e!=b&&(m.className.baseVal=b);return this};c.clone=function(){var c=x(this.node.cloneNode(!0));v(c.node,"id")&&v(c.node,{id:c.id});m(c);c.insertAfter(this);return c};c.toDefs=function(){u(this).appendChild(this.node);return this};c.pattern=c.toPattern=function(c,
a,b,m){var e=r("pattern",u(this));null==c&&(c=this.getBBox());y(c,"object")&&"x"in c&&(a=c.y,b=c.width,m=c.height,c=c.x);v(e.node,{x:c,y:a,width:b,height:m,patternUnits:"userSpaceOnUse",id:e.id,viewBox:[c,a,b,m].join(" ")});e.node.appendChild(this.node);return e};c.marker=function(c,a,b,m,e,h){var d=r("marker",u(this));null==c&&(c=this.getBBox());y(c,"object")&&"x"in c&&(a=c.y,b=c.width,m=c.height,e=c.refX||c.cx,h=c.refY||c.cy,c=c.x);v(d.node,{viewBox:[c,a,b,m].join(" "),markerWidth:b,markerHeight:m,
orient:"auto",refX:e||0,refY:h||0,id:d.id});d.node.appendChild(this.node);return d};var E=function(c,a,b,m){"function"!=typeof b||b.length||(m=b,b=L.linear);this.attr=c;this.dur=a;b&&(this.easing=b);m&&(this.callback=m)};a._.Animation=E;a.animation=function(c,a,b,m){return new E(c,a,b,m)};c.inAnim=function(){var c=[],a;for(a in this.anims)this.anims[h](a)&&function(a){c.push({anim:new E(a._attrs,a.dur,a.easing,a._callback),mina:a,curStatus:a.status(),status:function(c){return a.status(c)},stop:function(){a.stop()}})}(this.anims[a]);
return c};a.animate=function(c,a,b,m,e,h){"function"!=typeof e||e.length||(h=e,e=L.linear);var d=L.time();c=L(c,a,d,d+m,L.time,b,e);h&&k.once("mina.finish."+c.id,h);return c};c.stop=function(){for(var c=this.inAnim(),a=0,b=c.length;a<b;a++)c[a].stop();return this};c.animate=function(c,a,b,m){"function"!=typeof b||b.length||(m=b,b=L.linear);c instanceof E&&(m=c.callback,b=c.easing,a=b.dur,c=c.attr);var d=[],f=[],l={},t,ca,n,T=this,q;for(q in c)if(c[h](q)){T.equal?(n=T.equal(q,J(c[q])),t=n.from,ca=
n.to,n=n.f):(t=+T.attr(q),ca=+c[q]);var la=y(t,"array")?t.length:1;l[q]=e(d.length,d.length+la,n);d=d.concat(t);f=f.concat(ca)}t=L.time();var p=L(d,f,t,t+a,L.time,function(c){var a={},b;for(b in l)l[h](b)&&(a[b]=l[b](c));T.attr(a)},b);T.anims[p.id]=p;p._attrs=c;p._callback=m;k("snap.animcreated."+T.id,p);k.once("mina.finish."+p.id,function(){delete T.anims[p.id];m&&m.call(T)});k.once("mina.stop."+p.id,function(){delete T.anims[p.id]});return T};var T={};c.data=function(c,b){var m=T[this.id]=T[this.id]||
{};if(0==arguments.length)return k("snap.data.get."+this.id,this,m,null),m;if(1==arguments.length){if(a.is(c,"object")){for(var e in c)c[h](e)&&this.data(e,c[e]);return this}k("snap.data.get."+this.id,this,m[c],c);return m[c]}m[c]=b;k("snap.data.set."+this.id,this,b,c);return this};c.removeData=function(c){null==c?T[this.id]={}:T[this.id]&&delete T[this.id][c];return this};c.outerSVG=c.toString=d(1);c.innerSVG=d()})(e.prototype);a.parse=function(c){var a=G.doc.createDocumentFragment(),b=!0,m=G.doc.createElement("div");
c=J(c);c.match(/^\s*<\s*svg(?:\s|>)/)||(c="<svg>"+c+"</svg>",b=!1);m.innerHTML=c;if(c=m.getElementsByTagName("svg")[0])if(b)a=c;else for(;c.firstChild;)a.appendChild(c.firstChild);m.innerHTML=aa;return new l(a)};l.prototype.select=e.prototype.select;l.prototype.selectAll=e.prototype.selectAll;a.fragment=function(){for(var c=Array.prototype.slice.call(arguments,0),b=G.doc.createDocumentFragment(),m=0,e=c.length;m<e;m++){var h=c[m];h.node&&h.node.nodeType&&b.appendChild(h.node);h.nodeType&&b.appendChild(h);
"string"==typeof h&&b.appendChild(a.parse(h).node)}return new l(b)};a._.make=r;a._.wrap=x;s.prototype.el=function(c,a){var b=r(c,this.node);a&&b.attr(a);return b};k.on("snap.util.getattr",function(){var c=k.nt(),c=c.substring(c.lastIndexOf(".")+1),a=c.replace(/[A-Z]/g,function(c){return"-"+c.toLowerCase()});return pa[h](a)?this.node.ownerDocument.defaultView.getComputedStyle(this.node,null).getPropertyValue(a):v(this.node,c)});var pa={"alignment-baseline":0,"baseline-shift":0,clip:0,"clip-path":0,
"clip-rule":0,color:0,"color-interpolation":0,"color-interpolation-filters":0,"color-profile":0,"color-rendering":0,cursor:0,direction:0,display:0,"dominant-baseline":0,"enable-background":0,fill:0,"fill-opacity":0,"fill-rule":0,filter:0,"flood-color":0,"flood-opacity":0,font:0,"font-family":0,"font-size":0,"font-size-adjust":0,"font-stretch":0,"font-style":0,"font-variant":0,"font-weight":0,"glyph-orientation-horizontal":0,"glyph-orientation-vertical":0,"image-rendering":0,kerning:0,"letter-spacing":0,
"lighting-color":0,marker:0,"marker-end":0,"marker-mid":0,"marker-start":0,mask:0,opacity:0,overflow:0,"pointer-events":0,"shape-rendering":0,"stop-color":0,"stop-opacity":0,stroke:0,"stroke-dasharray":0,"stroke-dashoffset":0,"stroke-linecap":0,"stroke-linejoin":0,"stroke-miterlimit":0,"stroke-opacity":0,"stroke-width":0,"text-anchor":0,"text-decoration":0,"text-rendering":0,"unicode-bidi":0,visibility:0,"word-spacing":0,"writing-mode":0};k.on("snap.util.attr",function(c){var a=k.nt(),b={},a=a.substring(a.lastIndexOf(".")+
1);b[a]=c;var m=a.replace(/-(\w)/gi,function(c,a){return a.toUpperCase()}),a=a.replace(/[A-Z]/g,function(c){return"-"+c.toLowerCase()});pa[h](a)?this.node.style[m]=null==c?aa:c:v(this.node,b)});a.ajax=function(c,a,b,m){var e=new XMLHttpRequest,h=V();if(e){if(y(a,"function"))m=b,b=a,a=null;else if(y(a,"object")){var d=[],f;for(f in a)a.hasOwnProperty(f)&&d.push(encodeURIComponent(f)+"="+encodeURIComponent(a[f]));a=d.join("&")}e.open(a?"POST":"GET",c,!0);a&&(e.setRequestHeader("X-Requested-With","XMLHttpRequest"),
e.setRequestHeader("Content-type","application/x-www-form-urlencoded"));b&&(k.once("snap.ajax."+h+".0",b),k.once("snap.ajax."+h+".200",b),k.once("snap.ajax."+h+".304",b));e.onreadystatechange=function(){4==e.readyState&&k("snap.ajax."+h+"."+e.status,m,e)};if(4==e.readyState)return e;e.send(a);return e}};a.load=function(c,b,m){a.ajax(c,function(c){c=a.parse(c.responseText);m?b.call(m,c):b(c)})};a.getElementByPoint=function(c,a){var b,m,e=G.doc.elementFromPoint(c,a);if(G.win.opera&&"svg"==e.tagName){b=
e;m=b.getBoundingClientRect();b=b.ownerDocument;var h=b.body,d=b.documentElement;b=m.top+(g.win.pageYOffset||d.scrollTop||h.scrollTop)-(d.clientTop||h.clientTop||0);m=m.left+(g.win.pageXOffset||d.scrollLeft||h.scrollLeft)-(d.clientLeft||h.clientLeft||0);h=e.createSVGRect();h.x=c-m;h.y=a-b;h.width=h.height=1;b=e.getIntersectionList(h,null);b.length&&(e=b[b.length-1])}return e?x(e):null};a.plugin=function(c){c(a,e,s,G,l)};return G.win.Snap=a}();C.plugin(function(a,k,y,M,A){function w(a,d,f,b,q,e){null==
d&&"[object SVGMatrix]"==z.call(a)?(this.a=a.a,this.b=a.b,this.c=a.c,this.d=a.d,this.e=a.e,this.f=a.f):null!=a?(this.a=+a,this.b=+d,this.c=+f,this.d=+b,this.e=+q,this.f=+e):(this.a=1,this.c=this.b=0,this.d=1,this.f=this.e=0)}var z=Object.prototype.toString,d=String,f=Math;(function(n){function k(a){return a[0]*a[0]+a[1]*a[1]}function p(a){var d=f.sqrt(k(a));a[0]&&(a[0]/=d);a[1]&&(a[1]/=d)}n.add=function(a,d,e,f,n,p){var k=[[],[],[] ],u=[[this.a,this.c,this.e],[this.b,this.d,this.f],[0,0,1] ];d=[[a,
e,n],[d,f,p],[0,0,1] ];a&&a instanceof w&&(d=[[a.a,a.c,a.e],[a.b,a.d,a.f],[0,0,1] ]);for(a=0;3>a;a++)for(e=0;3>e;e++){for(f=n=0;3>f;f++)n+=u[a][f]*d[f][e];k[a][e]=n}this.a=k[0][0];this.b=k[1][0];this.c=k[0][1];this.d=k[1][1];this.e=k[0][2];this.f=k[1][2];return this};n.invert=function(){var a=this.a*this.d-this.b*this.c;return new w(this.d/a,-this.b/a,-this.c/a,this.a/a,(this.c*this.f-this.d*this.e)/a,(this.b*this.e-this.a*this.f)/a)};n.clone=function(){return new w(this.a,this.b,this.c,this.d,this.e,
this.f)};n.translate=function(a,d){return this.add(1,0,0,1,a,d)};n.scale=function(a,d,e,f){null==d&&(d=a);(e||f)&&this.add(1,0,0,1,e,f);this.add(a,0,0,d,0,0);(e||f)&&this.add(1,0,0,1,-e,-f);return this};n.rotate=function(b,d,e){b=a.rad(b);d=d||0;e=e||0;var l=+f.cos(b).toFixed(9);b=+f.sin(b).toFixed(9);this.add(l,b,-b,l,d,e);return this.add(1,0,0,1,-d,-e)};n.x=function(a,d){return a*this.a+d*this.c+this.e};n.y=function(a,d){return a*this.b+d*this.d+this.f};n.get=function(a){return+this[d.fromCharCode(97+
a)].toFixed(4)};n.toString=function(){return"matrix("+[this.get(0),this.get(1),this.get(2),this.get(3),this.get(4),this.get(5)].join()+")"};n.offset=function(){return[this.e.toFixed(4),this.f.toFixed(4)]};n.determinant=function(){return this.a*this.d-this.b*this.c};n.split=function(){var b={};b.dx=this.e;b.dy=this.f;var d=[[this.a,this.c],[this.b,this.d] ];b.scalex=f.sqrt(k(d[0]));p(d[0]);b.shear=d[0][0]*d[1][0]+d[0][1]*d[1][1];d[1]=[d[1][0]-d[0][0]*b.shear,d[1][1]-d[0][1]*b.shear];b.scaley=f.sqrt(k(d[1]));
p(d[1]);b.shear/=b.scaley;0>this.determinant()&&(b.scalex=-b.scalex);var e=-d[0][1],d=d[1][1];0>d?(b.rotate=a.deg(f.acos(d)),0>e&&(b.rotate=360-b.rotate)):b.rotate=a.deg(f.asin(e));b.isSimple=!+b.shear.toFixed(9)&&(b.scalex.toFixed(9)==b.scaley.toFixed(9)||!b.rotate);b.isSuperSimple=!+b.shear.toFixed(9)&&b.scalex.toFixed(9)==b.scaley.toFixed(9)&&!b.rotate;b.noRotation=!+b.shear.toFixed(9)&&!b.rotate;return b};n.toTransformString=function(a){a=a||this.split();if(+a.shear.toFixed(9))return"m"+[this.get(0),
this.get(1),this.get(2),this.get(3),this.get(4),this.get(5)];a.scalex=+a.scalex.toFixed(4);a.scaley=+a.scaley.toFixed(4);a.rotate=+a.rotate.toFixed(4);return(a.dx||a.dy?"t"+[+a.dx.toFixed(4),+a.dy.toFixed(4)]:"")+(1!=a.scalex||1!=a.scaley?"s"+[a.scalex,a.scaley,0,0]:"")+(a.rotate?"r"+[+a.rotate.toFixed(4),0,0]:"")}})(w.prototype);a.Matrix=w;a.matrix=function(a,d,f,b,k,e){return new w(a,d,f,b,k,e)}});C.plugin(function(a,v,y,M,A){function w(h){return function(d){k.stop();d instanceof A&&1==d.node.childNodes.length&&
("radialGradient"==d.node.firstChild.tagName||"linearGradient"==d.node.firstChild.tagName||"pattern"==d.node.firstChild.tagName)&&(d=d.node.firstChild,b(this).appendChild(d),d=u(d));if(d instanceof v)if("radialGradient"==d.type||"linearGradient"==d.type||"pattern"==d.type){d.node.id||e(d.node,{id:d.id});var f=l(d.node.id)}else f=d.attr(h);else f=a.color(d),f.error?(f=a(b(this).ownerSVGElement).gradient(d))?(f.node.id||e(f.node,{id:f.id}),f=l(f.node.id)):f=d:f=r(f);d={};d[h]=f;e(this.node,d);this.node.style[h]=
x}}function z(a){k.stop();a==+a&&(a+="px");this.node.style.fontSize=a}function d(a){var b=[];a=a.childNodes;for(var e=0,f=a.length;e<f;e++){var l=a[e];3==l.nodeType&&b.push(l.nodeValue);"tspan"==l.tagName&&(1==l.childNodes.length&&3==l.firstChild.nodeType?b.push(l.firstChild.nodeValue):b.push(d(l)))}return b}function f(){k.stop();return this.node.style.fontSize}var n=a._.make,u=a._.wrap,p=a.is,b=a._.getSomeDefs,q=/^url\(#?([^)]+)\)$/,e=a._.$,l=a.url,r=String,s=a._.separator,x="";k.on("snap.util.attr.mask",
function(a){if(a instanceof v||a instanceof A){k.stop();a instanceof A&&1==a.node.childNodes.length&&(a=a.node.firstChild,b(this).appendChild(a),a=u(a));if("mask"==a.type)var d=a;else d=n("mask",b(this)),d.node.appendChild(a.node);!d.node.id&&e(d.node,{id:d.id});e(this.node,{mask:l(d.id)})}});(function(a){k.on("snap.util.attr.clip",a);k.on("snap.util.attr.clip-path",a);k.on("snap.util.attr.clipPath",a)})(function(a){if(a instanceof v||a instanceof A){k.stop();if("clipPath"==a.type)var d=a;else d=
n("clipPath",b(this)),d.node.appendChild(a.node),!d.node.id&&e(d.node,{id:d.id});e(this.node,{"clip-path":l(d.id)})}});k.on("snap.util.attr.fill",w("fill"));k.on("snap.util.attr.stroke",w("stroke"));var G=/^([lr])(?:\(([^)]*)\))?(.*)$/i;k.on("snap.util.grad.parse",function(a){a=r(a);var b=a.match(G);if(!b)return null;a=b[1];var e=b[2],b=b[3],e=e.split(/\s*,\s*/).map(function(a){return+a==a?+a:a});1==e.length&&0==e[0]&&(e=[]);b=b.split("-");b=b.map(function(a){a=a.split(":");var b={color:a[0]};a[1]&&
(b.offset=parseFloat(a[1]));return b});return{type:a,params:e,stops:b}});k.on("snap.util.attr.d",function(b){k.stop();p(b,"array")&&p(b[0],"array")&&(b=a.path.toString.call(b));b=r(b);b.match(/[ruo]/i)&&(b=a.path.toAbsolute(b));e(this.node,{d:b})})(-1);k.on("snap.util.attr.#text",function(a){k.stop();a=r(a);for(a=M.doc.createTextNode(a);this.node.firstChild;)this.node.removeChild(this.node.firstChild);this.node.appendChild(a)})(-1);k.on("snap.util.attr.path",function(a){k.stop();this.attr({d:a})})(-1);
k.on("snap.util.attr.class",function(a){k.stop();this.node.className.baseVal=a})(-1);k.on("snap.util.attr.viewBox",function(a){a=p(a,"object")&&"x"in a?[a.x,a.y,a.width,a.height].join(" "):p(a,"array")?a.join(" "):a;e(this.node,{viewBox:a});k.stop()})(-1);k.on("snap.util.attr.transform",function(a){this.transform(a);k.stop()})(-1);k.on("snap.util.attr.r",function(a){"rect"==this.type&&(k.stop(),e(this.node,{rx:a,ry:a}))})(-1);k.on("snap.util.attr.textpath",function(a){k.stop();if("text"==this.type){var d,
f;if(!a&&this.textPath){for(a=this.textPath;a.node.firstChild;)this.node.appendChild(a.node.firstChild);a.remove();delete this.textPath}else if(p(a,"string")?(d=b(this),a=u(d.parentNode).path(a),d.appendChild(a.node),d=a.id,a.attr({id:d})):(a=u(a),a instanceof v&&(d=a.attr("id"),d||(d=a.id,a.attr({id:d})))),d)if(a=this.textPath,f=this.node,a)a.attr({"xlink:href":"#"+d});else{for(a=e("textPath",{"xlink:href":"#"+d});f.firstChild;)a.appendChild(f.firstChild);f.appendChild(a);this.textPath=u(a)}}})(-1);
k.on("snap.util.attr.text",function(a){if("text"==this.type){for(var b=this.node,d=function(a){var b=e("tspan");if(p(a,"array"))for(var f=0;f<a.length;f++)b.appendChild(d(a[f]));else b.appendChild(M.doc.createTextNode(a));b.normalize&&b.normalize();return b};b.firstChild;)b.removeChild(b.firstChild);for(a=d(a);a.firstChild;)b.appendChild(a.firstChild)}k.stop()})(-1);k.on("snap.util.attr.fontSize",z)(-1);k.on("snap.util.attr.font-size",z)(-1);k.on("snap.util.getattr.transform",function(){k.stop();
return this.transform()})(-1);k.on("snap.util.getattr.textpath",function(){k.stop();return this.textPath})(-1);(function(){function b(d){return function(){k.stop();var b=M.doc.defaultView.getComputedStyle(this.node,null).getPropertyValue("marker-"+d);return"none"==b?b:a(M.doc.getElementById(b.match(q)[1]))}}function d(a){return function(b){k.stop();var d="marker"+a.charAt(0).toUpperCase()+a.substring(1);if(""==b||!b)this.node.style[d]="none";else if("marker"==b.type){var f=b.node.id;f||e(b.node,{id:b.id});
this.node.style[d]=l(f)}}}k.on("snap.util.getattr.marker-end",b("end"))(-1);k.on("snap.util.getattr.markerEnd",b("end"))(-1);k.on("snap.util.getattr.marker-start",b("start"))(-1);k.on("snap.util.getattr.markerStart",b("start"))(-1);k.on("snap.util.getattr.marker-mid",b("mid"))(-1);k.on("snap.util.getattr.markerMid",b("mid"))(-1);k.on("snap.util.attr.marker-end",d("end"))(-1);k.on("snap.util.attr.markerEnd",d("end"))(-1);k.on("snap.util.attr.marker-start",d("start"))(-1);k.on("snap.util.attr.markerStart",
d("start"))(-1);k.on("snap.util.attr.marker-mid",d("mid"))(-1);k.on("snap.util.attr.markerMid",d("mid"))(-1)})();k.on("snap.util.getattr.r",function(){if("rect"==this.type&&e(this.node,"rx")==e(this.node,"ry"))return k.stop(),e(this.node,"rx")})(-1);k.on("snap.util.getattr.text",function(){if("text"==this.type||"tspan"==this.type){k.stop();var a=d(this.node);return 1==a.length?a[0]:a}})(-1);k.on("snap.util.getattr.#text",function(){return this.node.textContent})(-1);k.on("snap.util.getattr.viewBox",
function(){k.stop();var b=e(this.node,"viewBox");if(b)return b=b.split(s),a._.box(+b[0],+b[1],+b[2],+b[3])})(-1);k.on("snap.util.getattr.points",function(){var a=e(this.node,"points");k.stop();if(a)return a.split(s)})(-1);k.on("snap.util.getattr.path",function(){var a=e(this.node,"d");k.stop();return a})(-1);k.on("snap.util.getattr.class",function(){return this.node.className.baseVal})(-1);k.on("snap.util.getattr.fontSize",f)(-1);k.on("snap.util.getattr.font-size",f)(-1)});C.plugin(function(a,v,y,
M,A){function w(a){return a}function z(a){return function(b){return+b.toFixed(3)+a}}var d={"+":function(a,b){return a+b},"-":function(a,b){return a-b},"/":function(a,b){return a/b},"*":function(a,b){return a*b}},f=String,n=/[a-z]+$/i,u=/^\s*([+\-\/*])\s*=\s*([\d.eE+\-]+)\s*([^\d\s]+)?\s*$/;k.on("snap.util.attr",function(a){if(a=f(a).match(u)){var b=k.nt(),b=b.substring(b.lastIndexOf(".")+1),q=this.attr(b),e={};k.stop();var l=a[3]||"",r=q.match(n),s=d[a[1] ];r&&r==l?a=s(parseFloat(q),+a[2]):(q=this.asPX(b),
a=s(this.asPX(b),this.asPX(b,a[2]+l)));isNaN(q)||isNaN(a)||(e[b]=a,this.attr(e))}})(-10);k.on("snap.util.equal",function(a,b){var q=f(this.attr(a)||""),e=f(b).match(u);if(e){k.stop();var l=e[3]||"",r=q.match(n),s=d[e[1] ];if(r&&r==l)return{from:parseFloat(q),to:s(parseFloat(q),+e[2]),f:z(r)};q=this.asPX(a);return{from:q,to:s(q,this.asPX(a,e[2]+l)),f:w}}})(-10)});C.plugin(function(a,v,y,M,A){var w=y.prototype,z=a.is;w.rect=function(a,d,k,p,b,q){var e;null==q&&(q=b);z(a,"object")&&"[object Object]"==
a?e=a:null!=a&&(e={x:a,y:d,width:k,height:p},null!=b&&(e.rx=b,e.ry=q));return this.el("rect",e)};w.circle=function(a,d,k){var p;z(a,"object")&&"[object Object]"==a?p=a:null!=a&&(p={cx:a,cy:d,r:k});return this.el("circle",p)};var d=function(){function a(){this.parentNode.removeChild(this)}return function(d,k){var p=M.doc.createElement("img"),b=M.doc.body;p.style.cssText="position:absolute;left:-9999em;top:-9999em";p.onload=function(){k.call(p);p.onload=p.onerror=null;b.removeChild(p)};p.onerror=a;
b.appendChild(p);p.src=d}}();w.image=function(f,n,k,p,b){var q=this.el("image");if(z(f,"object")&&"src"in f)q.attr(f);else if(null!=f){var e={"xlink:href":f,preserveAspectRatio:"none"};null!=n&&null!=k&&(e.x=n,e.y=k);null!=p&&null!=b?(e.width=p,e.height=b):d(f,function(){a._.$(q.node,{width:this.offsetWidth,height:this.offsetHeight})});a._.$(q.node,e)}return q};w.ellipse=function(a,d,k,p){var b;z(a,"object")&&"[object Object]"==a?b=a:null!=a&&(b={cx:a,cy:d,rx:k,ry:p});return this.el("ellipse",b)};
w.path=function(a){var d;z(a,"object")&&!z(a,"array")?d=a:a&&(d={d:a});return this.el("path",d)};w.group=w.g=function(a){var d=this.el("g");1==arguments.length&&a&&!a.type?d.attr(a):arguments.length&&d.add(Array.prototype.slice.call(arguments,0));return d};w.svg=function(a,d,k,p,b,q,e,l){var r={};z(a,"object")&&null==d?r=a:(null!=a&&(r.x=a),null!=d&&(r.y=d),null!=k&&(r.width=k),null!=p&&(r.height=p),null!=b&&null!=q&&null!=e&&null!=l&&(r.viewBox=[b,q,e,l]));return this.el("svg",r)};w.mask=function(a){var d=
this.el("mask");1==arguments.length&&a&&!a.type?d.attr(a):arguments.length&&d.add(Array.prototype.slice.call(arguments,0));return d};w.ptrn=function(a,d,k,p,b,q,e,l){if(z(a,"object"))var r=a;else arguments.length?(r={},null!=a&&(r.x=a),null!=d&&(r.y=d),null!=k&&(r.width=k),null!=p&&(r.height=p),null!=b&&null!=q&&null!=e&&null!=l&&(r.viewBox=[b,q,e,l])):r={patternUnits:"userSpaceOnUse"};return this.el("pattern",r)};w.use=function(a){return null!=a?(make("use",this.node),a instanceof v&&(a.attr("id")||
a.attr({id:ID()}),a=a.attr("id")),this.el("use",{"xlink:href":a})):v.prototype.use.call(this)};w.text=function(a,d,k){var p={};z(a,"object")?p=a:null!=a&&(p={x:a,y:d,text:k||""});return this.el("text",p)};w.line=function(a,d,k,p){var b={};z(a,"object")?b=a:null!=a&&(b={x1:a,x2:k,y1:d,y2:p});return this.el("line",b)};w.polyline=function(a){1<arguments.length&&(a=Array.prototype.slice.call(arguments,0));var d={};z(a,"object")&&!z(a,"array")?d=a:null!=a&&(d={points:a});return this.el("polyline",d)};
w.polygon=function(a){1<arguments.length&&(a=Array.prototype.slice.call(arguments,0));var d={};z(a,"object")&&!z(a,"array")?d=a:null!=a&&(d={points:a});return this.el("polygon",d)};(function(){function d(){return this.selectAll("stop")}function n(b,d){var f=e("stop"),k={offset:+d+"%"};b=a.color(b);k["stop-color"]=b.hex;1>b.opacity&&(k["stop-opacity"]=b.opacity);e(f,k);this.node.appendChild(f);return this}function u(){if("linearGradient"==this.type){var b=e(this.node,"x1")||0,d=e(this.node,"x2")||
1,f=e(this.node,"y1")||0,k=e(this.node,"y2")||0;return a._.box(b,f,math.abs(d-b),math.abs(k-f))}b=this.node.r||0;return a._.box((this.node.cx||0.5)-b,(this.node.cy||0.5)-b,2*b,2*b)}function p(a,d){function f(a,b){for(var d=(b-u)/(a-w),e=w;e<a;e++)h[e].offset=+(+u+d*(e-w)).toFixed(2);w=a;u=b}var n=k("snap.util.grad.parse",null,d).firstDefined(),p;if(!n)return null;n.params.unshift(a);p="l"==n.type.toLowerCase()?b.apply(0,n.params):q.apply(0,n.params);n.type!=n.type.toLowerCase()&&e(p.node,{gradientUnits:"userSpaceOnUse"});
var h=n.stops,n=h.length,u=0,w=0;n--;for(var v=0;v<n;v++)"offset"in h[v]&&f(v,h[v].offset);h[n].offset=h[n].offset||100;f(n,h[n].offset);for(v=0;v<=n;v++){var y=h[v];p.addStop(y.color,y.offset)}return p}function b(b,k,p,q,w){b=a._.make("linearGradient",b);b.stops=d;b.addStop=n;b.getBBox=u;null!=k&&e(b.node,{x1:k,y1:p,x2:q,y2:w});return b}function q(b,k,p,q,w,h){b=a._.make("radialGradient",b);b.stops=d;b.addStop=n;b.getBBox=u;null!=k&&e(b.node,{cx:k,cy:p,r:q});null!=w&&null!=h&&e(b.node,{fx:w,fy:h});
return b}var e=a._.$;w.gradient=function(a){return p(this.defs,a)};w.gradientLinear=function(a,d,e,f){return b(this.defs,a,d,e,f)};w.gradientRadial=function(a,b,d,e,f){return q(this.defs,a,b,d,e,f)};w.toString=function(){var b=this.node.ownerDocument,d=b.createDocumentFragment(),b=b.createElement("div"),e=this.node.cloneNode(!0);d.appendChild(b);b.appendChild(e);a._.$(e,{xmlns:"http://www.w3.org/2000/svg"});b=b.innerHTML;d.removeChild(d.firstChild);return b};w.clear=function(){for(var a=this.node.firstChild,
b;a;)b=a.nextSibling,"defs"!=a.tagName?a.parentNode.removeChild(a):w.clear.call({node:a}),a=b}})()});C.plugin(function(a,k,y,M){function A(a){var b=A.ps=A.ps||{};b[a]?b[a].sleep=100:b[a]={sleep:100};setTimeout(function(){for(var d in b)b[L](d)&&d!=a&&(b[d].sleep--,!b[d].sleep&&delete b[d])});return b[a]}function w(a,b,d,e){null==a&&(a=b=d=e=0);null==b&&(b=a.y,d=a.width,e=a.height,a=a.x);return{x:a,y:b,width:d,w:d,height:e,h:e,x2:a+d,y2:b+e,cx:a+d/2,cy:b+e/2,r1:F.min(d,e)/2,r2:F.max(d,e)/2,r0:F.sqrt(d*
d+e*e)/2,path:s(a,b,d,e),vb:[a,b,d,e].join(" ")}}function z(){return this.join(",").replace(N,"$1")}function d(a){a=C(a);a.toString=z;return a}function f(a,b,d,h,f,k,l,n,p){if(null==p)return e(a,b,d,h,f,k,l,n);if(0>p||e(a,b,d,h,f,k,l,n)<p)p=void 0;else{var q=0.5,O=1-q,s;for(s=e(a,b,d,h,f,k,l,n,O);0.01<Z(s-p);)q/=2,O+=(s<p?1:-1)*q,s=e(a,b,d,h,f,k,l,n,O);p=O}return u(a,b,d,h,f,k,l,n,p)}function n(b,d){function e(a){return+(+a).toFixed(3)}return a._.cacher(function(a,h,l){a instanceof k&&(a=a.attr("d"));
a=I(a);for(var n,p,D,q,O="",s={},c=0,t=0,r=a.length;t<r;t++){D=a[t];if("M"==D[0])n=+D[1],p=+D[2];else{q=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6]);if(c+q>h){if(d&&!s.start){n=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6],h-c);O+=["C"+e(n.start.x),e(n.start.y),e(n.m.x),e(n.m.y),e(n.x),e(n.y)];if(l)return O;s.start=O;O=["M"+e(n.x),e(n.y)+"C"+e(n.n.x),e(n.n.y),e(n.end.x),e(n.end.y),e(D[5]),e(D[6])].join();c+=q;n=+D[5];p=+D[6];continue}if(!b&&!d)return n=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6],h-c)}c+=q;n=+D[5];p=+D[6]}O+=
D.shift()+D}s.end=O;return n=b?c:d?s:u(n,p,D[0],D[1],D[2],D[3],D[4],D[5],1)},null,a._.clone)}function u(a,b,d,e,h,f,k,l,n){var p=1-n,q=ma(p,3),s=ma(p,2),c=n*n,t=c*n,r=q*a+3*s*n*d+3*p*n*n*h+t*k,q=q*b+3*s*n*e+3*p*n*n*f+t*l,s=a+2*n*(d-a)+c*(h-2*d+a),t=b+2*n*(e-b)+c*(f-2*e+b),x=d+2*n*(h-d)+c*(k-2*h+d),c=e+2*n*(f-e)+c*(l-2*f+e);a=p*a+n*d;b=p*b+n*e;h=p*h+n*k;f=p*f+n*l;l=90-180*F.atan2(s-x,t-c)/S;return{x:r,y:q,m:{x:s,y:t},n:{x:x,y:c},start:{x:a,y:b},end:{x:h,y:f},alpha:l}}function p(b,d,e,h,f,n,k,l){a.is(b,
"array")||(b=[b,d,e,h,f,n,k,l]);b=U.apply(null,b);return w(b.min.x,b.min.y,b.max.x-b.min.x,b.max.y-b.min.y)}function b(a,b,d){return b>=a.x&&b<=a.x+a.width&&d>=a.y&&d<=a.y+a.height}function q(a,d){a=w(a);d=w(d);return b(d,a.x,a.y)||b(d,a.x2,a.y)||b(d,a.x,a.y2)||b(d,a.x2,a.y2)||b(a,d.x,d.y)||b(a,d.x2,d.y)||b(a,d.x,d.y2)||b(a,d.x2,d.y2)||(a.x<d.x2&&a.x>d.x||d.x<a.x2&&d.x>a.x)&&(a.y<d.y2&&a.y>d.y||d.y<a.y2&&d.y>a.y)}function e(a,b,d,e,h,f,n,k,l){null==l&&(l=1);l=(1<l?1:0>l?0:l)/2;for(var p=[-0.1252,
0.1252,-0.3678,0.3678,-0.5873,0.5873,-0.7699,0.7699,-0.9041,0.9041,-0.9816,0.9816],q=[0.2491,0.2491,0.2335,0.2335,0.2032,0.2032,0.1601,0.1601,0.1069,0.1069,0.0472,0.0472],s=0,c=0;12>c;c++)var t=l*p[c]+l,r=t*(t*(-3*a+9*d-9*h+3*n)+6*a-12*d+6*h)-3*a+3*d,t=t*(t*(-3*b+9*e-9*f+3*k)+6*b-12*e+6*f)-3*b+3*e,s=s+q[c]*F.sqrt(r*r+t*t);return l*s}function l(a,b,d){a=I(a);b=I(b);for(var h,f,l,n,k,s,r,O,x,c,t=d?0:[],w=0,v=a.length;w<v;w++)if(x=a[w],"M"==x[0])h=k=x[1],f=s=x[2];else{"C"==x[0]?(x=[h,f].concat(x.slice(1)),
h=x[6],f=x[7]):(x=[h,f,h,f,k,s,k,s],h=k,f=s);for(var G=0,y=b.length;G<y;G++)if(c=b[G],"M"==c[0])l=r=c[1],n=O=c[2];else{"C"==c[0]?(c=[l,n].concat(c.slice(1)),l=c[6],n=c[7]):(c=[l,n,l,n,r,O,r,O],l=r,n=O);var z;var K=x,B=c;z=d;var H=p(K),J=p(B);if(q(H,J)){for(var H=e.apply(0,K),J=e.apply(0,B),H=~~(H/8),J=~~(J/8),U=[],A=[],F={},M=z?0:[],P=0;P<H+1;P++){var C=u.apply(0,K.concat(P/H));U.push({x:C.x,y:C.y,t:P/H})}for(P=0;P<J+1;P++)C=u.apply(0,B.concat(P/J)),A.push({x:C.x,y:C.y,t:P/J});for(P=0;P<H;P++)for(K=
0;K<J;K++){var Q=U[P],L=U[P+1],B=A[K],C=A[K+1],N=0.001>Z(L.x-Q.x)?"y":"x",S=0.001>Z(C.x-B.x)?"y":"x",R;R=Q.x;var Y=Q.y,V=L.x,ea=L.y,fa=B.x,ga=B.y,ha=C.x,ia=C.y;if(W(R,V)<X(fa,ha)||X(R,V)>W(fa,ha)||W(Y,ea)<X(ga,ia)||X(Y,ea)>W(ga,ia))R=void 0;else{var $=(R*ea-Y*V)*(fa-ha)-(R-V)*(fa*ia-ga*ha),aa=(R*ea-Y*V)*(ga-ia)-(Y-ea)*(fa*ia-ga*ha),ja=(R-V)*(ga-ia)-(Y-ea)*(fa-ha);if(ja){var $=$/ja,aa=aa/ja,ja=+$.toFixed(2),ba=+aa.toFixed(2);R=ja<+X(R,V).toFixed(2)||ja>+W(R,V).toFixed(2)||ja<+X(fa,ha).toFixed(2)||
ja>+W(fa,ha).toFixed(2)||ba<+X(Y,ea).toFixed(2)||ba>+W(Y,ea).toFixed(2)||ba<+X(ga,ia).toFixed(2)||ba>+W(ga,ia).toFixed(2)?void 0:{x:$,y:aa}}else R=void 0}R&&F[R.x.toFixed(4)]!=R.y.toFixed(4)&&(F[R.x.toFixed(4)]=R.y.toFixed(4),Q=Q.t+Z((R[N]-Q[N])/(L[N]-Q[N]))*(L.t-Q.t),B=B.t+Z((R[S]-B[S])/(C[S]-B[S]))*(C.t-B.t),0<=Q&&1>=Q&&0<=B&&1>=B&&(z?M++:M.push({x:R.x,y:R.y,t1:Q,t2:B})))}z=M}else z=z?0:[];if(d)t+=z;else{H=0;for(J=z.length;H<J;H++)z[H].segment1=w,z[H].segment2=G,z[H].bez1=x,z[H].bez2=c;t=t.concat(z)}}}return t}
function r(a){var b=A(a);if(b.bbox)return C(b.bbox);if(!a)return w();a=I(a);for(var d=0,e=0,h=[],f=[],l,n=0,k=a.length;n<k;n++)l=a[n],"M"==l[0]?(d=l[1],e=l[2],h.push(d),f.push(e)):(d=U(d,e,l[1],l[2],l[3],l[4],l[5],l[6]),h=h.concat(d.min.x,d.max.x),f=f.concat(d.min.y,d.max.y),d=l[5],e=l[6]);a=X.apply(0,h);l=X.apply(0,f);h=W.apply(0,h);f=W.apply(0,f);f=w(a,l,h-a,f-l);b.bbox=C(f);return f}function s(a,b,d,e,h){if(h)return[["M",+a+ +h,b],["l",d-2*h,0],["a",h,h,0,0,1,h,h],["l",0,e-2*h],["a",h,h,0,0,1,
-h,h],["l",2*h-d,0],["a",h,h,0,0,1,-h,-h],["l",0,2*h-e],["a",h,h,0,0,1,h,-h],["z"] ];a=[["M",a,b],["l",d,0],["l",0,e],["l",-d,0],["z"] ];a.toString=z;return a}function x(a,b,d,e,h){null==h&&null==e&&(e=d);a=+a;b=+b;d=+d;e=+e;if(null!=h){var f=Math.PI/180,l=a+d*Math.cos(-e*f);a+=d*Math.cos(-h*f);var n=b+d*Math.sin(-e*f);b+=d*Math.sin(-h*f);d=[["M",l,n],["A",d,d,0,+(180<h-e),0,a,b] ]}else d=[["M",a,b],["m",0,-e],["a",d,e,0,1,1,0,2*e],["a",d,e,0,1,1,0,-2*e],["z"] ];d.toString=z;return d}function G(b){var e=
A(b);if(e.abs)return d(e.abs);Q(b,"array")&&Q(b&&b[0],"array")||(b=a.parsePathString(b));if(!b||!b.length)return[["M",0,0] ];var h=[],f=0,l=0,n=0,k=0,p=0;"M"==b[0][0]&&(f=+b[0][1],l=+b[0][2],n=f,k=l,p++,h[0]=["M",f,l]);for(var q=3==b.length&&"M"==b[0][0]&&"R"==b[1][0].toUpperCase()&&"Z"==b[2][0].toUpperCase(),s,r,w=p,c=b.length;w<c;w++){h.push(s=[]);r=b[w];p=r[0];if(p!=p.toUpperCase())switch(s[0]=p.toUpperCase(),s[0]){case "A":s[1]=r[1];s[2]=r[2];s[3]=r[3];s[4]=r[4];s[5]=r[5];s[6]=+r[6]+f;s[7]=+r[7]+
l;break;case "V":s[1]=+r[1]+l;break;case "H":s[1]=+r[1]+f;break;case "R":for(var t=[f,l].concat(r.slice(1)),u=2,v=t.length;u<v;u++)t[u]=+t[u]+f,t[++u]=+t[u]+l;h.pop();h=h.concat(P(t,q));break;case "O":h.pop();t=x(f,l,r[1],r[2]);t.push(t[0]);h=h.concat(t);break;case "U":h.pop();h=h.concat(x(f,l,r[1],r[2],r[3]));s=["U"].concat(h[h.length-1].slice(-2));break;case "M":n=+r[1]+f,k=+r[2]+l;default:for(u=1,v=r.length;u<v;u++)s[u]=+r[u]+(u%2?f:l)}else if("R"==p)t=[f,l].concat(r.slice(1)),h.pop(),h=h.concat(P(t,
q)),s=["R"].concat(r.slice(-2));else if("O"==p)h.pop(),t=x(f,l,r[1],r[2]),t.push(t[0]),h=h.concat(t);else if("U"==p)h.pop(),h=h.concat(x(f,l,r[1],r[2],r[3])),s=["U"].concat(h[h.length-1].slice(-2));else for(t=0,u=r.length;t<u;t++)s[t]=r[t];p=p.toUpperCase();if("O"!=p)switch(s[0]){case "Z":f=+n;l=+k;break;case "H":f=s[1];break;case "V":l=s[1];break;case "M":n=s[s.length-2],k=s[s.length-1];default:f=s[s.length-2],l=s[s.length-1]}}h.toString=z;e.abs=d(h);return h}function h(a,b,d,e){return[a,b,d,e,d,
e]}function J(a,b,d,e,h,f){var l=1/3,n=2/3;return[l*a+n*d,l*b+n*e,l*h+n*d,l*f+n*e,h,f]}function K(b,d,e,h,f,l,n,k,p,s){var r=120*S/180,q=S/180*(+f||0),c=[],t,x=a._.cacher(function(a,b,c){var d=a*F.cos(c)-b*F.sin(c);a=a*F.sin(c)+b*F.cos(c);return{x:d,y:a}});if(s)v=s[0],t=s[1],l=s[2],u=s[3];else{t=x(b,d,-q);b=t.x;d=t.y;t=x(k,p,-q);k=t.x;p=t.y;F.cos(S/180*f);F.sin(S/180*f);t=(b-k)/2;v=(d-p)/2;u=t*t/(e*e)+v*v/(h*h);1<u&&(u=F.sqrt(u),e*=u,h*=u);var u=e*e,w=h*h,u=(l==n?-1:1)*F.sqrt(Z((u*w-u*v*v-w*t*t)/
(u*v*v+w*t*t)));l=u*e*v/h+(b+k)/2;var u=u*-h*t/e+(d+p)/2,v=F.asin(((d-u)/h).toFixed(9));t=F.asin(((p-u)/h).toFixed(9));v=b<l?S-v:v;t=k<l?S-t:t;0>v&&(v=2*S+v);0>t&&(t=2*S+t);n&&v>t&&(v-=2*S);!n&&t>v&&(t-=2*S)}if(Z(t-v)>r){var c=t,w=k,G=p;t=v+r*(n&&t>v?1:-1);k=l+e*F.cos(t);p=u+h*F.sin(t);c=K(k,p,e,h,f,0,n,w,G,[t,c,l,u])}l=t-v;f=F.cos(v);r=F.sin(v);n=F.cos(t);t=F.sin(t);l=F.tan(l/4);e=4/3*e*l;l*=4/3*h;h=[b,d];b=[b+e*r,d-l*f];d=[k+e*t,p-l*n];k=[k,p];b[0]=2*h[0]-b[0];b[1]=2*h[1]-b[1];if(s)return[b,d,k].concat(c);
c=[b,d,k].concat(c).join().split(",");s=[];k=0;for(p=c.length;k<p;k++)s[k]=k%2?x(c[k-1],c[k],q).y:x(c[k],c[k+1],q).x;return s}function U(a,b,d,e,h,f,l,k){for(var n=[],p=[[],[] ],s,r,c,t,q=0;2>q;++q)0==q?(r=6*a-12*d+6*h,s=-3*a+9*d-9*h+3*l,c=3*d-3*a):(r=6*b-12*e+6*f,s=-3*b+9*e-9*f+3*k,c=3*e-3*b),1E-12>Z(s)?1E-12>Z(r)||(s=-c/r,0<s&&1>s&&n.push(s)):(t=r*r-4*c*s,c=F.sqrt(t),0>t||(t=(-r+c)/(2*s),0<t&&1>t&&n.push(t),s=(-r-c)/(2*s),0<s&&1>s&&n.push(s)));for(r=q=n.length;q--;)s=n[q],c=1-s,p[0][q]=c*c*c*a+3*
c*c*s*d+3*c*s*s*h+s*s*s*l,p[1][q]=c*c*c*b+3*c*c*s*e+3*c*s*s*f+s*s*s*k;p[0][r]=a;p[1][r]=b;p[0][r+1]=l;p[1][r+1]=k;p[0].length=p[1].length=r+2;return{min:{x:X.apply(0,p[0]),y:X.apply(0,p[1])},max:{x:W.apply(0,p[0]),y:W.apply(0,p[1])}}}function I(a,b){var e=!b&&A(a);if(!b&&e.curve)return d(e.curve);var f=G(a),l=b&&G(b),n={x:0,y:0,bx:0,by:0,X:0,Y:0,qx:null,qy:null},k={x:0,y:0,bx:0,by:0,X:0,Y:0,qx:null,qy:null},p=function(a,b,c){if(!a)return["C",b.x,b.y,b.x,b.y,b.x,b.y];a[0]in{T:1,Q:1}||(b.qx=b.qy=null);
switch(a[0]){case "M":b.X=a[1];b.Y=a[2];break;case "A":a=["C"].concat(K.apply(0,[b.x,b.y].concat(a.slice(1))));break;case "S":"C"==c||"S"==c?(c=2*b.x-b.bx,b=2*b.y-b.by):(c=b.x,b=b.y);a=["C",c,b].concat(a.slice(1));break;case "T":"Q"==c||"T"==c?(b.qx=2*b.x-b.qx,b.qy=2*b.y-b.qy):(b.qx=b.x,b.qy=b.y);a=["C"].concat(J(b.x,b.y,b.qx,b.qy,a[1],a[2]));break;case "Q":b.qx=a[1];b.qy=a[2];a=["C"].concat(J(b.x,b.y,a[1],a[2],a[3],a[4]));break;case "L":a=["C"].concat(h(b.x,b.y,a[1],a[2]));break;case "H":a=["C"].concat(h(b.x,
b.y,a[1],b.y));break;case "V":a=["C"].concat(h(b.x,b.y,b.x,a[1]));break;case "Z":a=["C"].concat(h(b.x,b.y,b.X,b.Y))}return a},s=function(a,b){if(7<a[b].length){a[b].shift();for(var c=a[b];c.length;)q[b]="A",l&&(u[b]="A"),a.splice(b++,0,["C"].concat(c.splice(0,6)));a.splice(b,1);v=W(f.length,l&&l.length||0)}},r=function(a,b,c,d,e){a&&b&&"M"==a[e][0]&&"M"!=b[e][0]&&(b.splice(e,0,["M",d.x,d.y]),c.bx=0,c.by=0,c.x=a[e][1],c.y=a[e][2],v=W(f.length,l&&l.length||0))},q=[],u=[],c="",t="",x=0,v=W(f.length,
l&&l.length||0);for(;x<v;x++){f[x]&&(c=f[x][0]);"C"!=c&&(q[x]=c,x&&(t=q[x-1]));f[x]=p(f[x],n,t);"A"!=q[x]&&"C"==c&&(q[x]="C");s(f,x);l&&(l[x]&&(c=l[x][0]),"C"!=c&&(u[x]=c,x&&(t=u[x-1])),l[x]=p(l[x],k,t),"A"!=u[x]&&"C"==c&&(u[x]="C"),s(l,x));r(f,l,n,k,x);r(l,f,k,n,x);var w=f[x],z=l&&l[x],y=w.length,U=l&&z.length;n.x=w[y-2];n.y=w[y-1];n.bx=$(w[y-4])||n.x;n.by=$(w[y-3])||n.y;k.bx=l&&($(z[U-4])||k.x);k.by=l&&($(z[U-3])||k.y);k.x=l&&z[U-2];k.y=l&&z[U-1]}l||(e.curve=d(f));return l?[f,l]:f}function P(a,
b){for(var d=[],e=0,h=a.length;h-2*!b>e;e+=2){var f=[{x:+a[e-2],y:+a[e-1]},{x:+a[e],y:+a[e+1]},{x:+a[e+2],y:+a[e+3]},{x:+a[e+4],y:+a[e+5]}];b?e?h-4==e?f[3]={x:+a[0],y:+a[1]}:h-2==e&&(f[2]={x:+a[0],y:+a[1]},f[3]={x:+a[2],y:+a[3]}):f[0]={x:+a[h-2],y:+a[h-1]}:h-4==e?f[3]=f[2]:e||(f[0]={x:+a[e],y:+a[e+1]});d.push(["C",(-f[0].x+6*f[1].x+f[2].x)/6,(-f[0].y+6*f[1].y+f[2].y)/6,(f[1].x+6*f[2].x-f[3].x)/6,(f[1].y+6*f[2].y-f[3].y)/6,f[2].x,f[2].y])}return d}y=k.prototype;var Q=a.is,C=a._.clone,L="hasOwnProperty",
N=/,?([a-z]),?/gi,$=parseFloat,F=Math,S=F.PI,X=F.min,W=F.max,ma=F.pow,Z=F.abs;M=n(1);var na=n(),ba=n(0,1),V=a._unit2px;a.path=A;a.path.getTotalLength=M;a.path.getPointAtLength=na;a.path.getSubpath=function(a,b,d){if(1E-6>this.getTotalLength(a)-d)return ba(a,b).end;a=ba(a,d,1);return b?ba(a,b).end:a};y.getTotalLength=function(){if(this.node.getTotalLength)return this.node.getTotalLength()};y.getPointAtLength=function(a){return na(this.attr("d"),a)};y.getSubpath=function(b,d){return a.path.getSubpath(this.attr("d"),
b,d)};a._.box=w;a.path.findDotsAtSegment=u;a.path.bezierBBox=p;a.path.isPointInsideBBox=b;a.path.isBBoxIntersect=q;a.path.intersection=function(a,b){return l(a,b)};a.path.intersectionNumber=function(a,b){return l(a,b,1)};a.path.isPointInside=function(a,d,e){var h=r(a);return b(h,d,e)&&1==l(a,[["M",d,e],["H",h.x2+10] ],1)%2};a.path.getBBox=r;a.path.get={path:function(a){return a.attr("path")},circle:function(a){a=V(a);return x(a.cx,a.cy,a.r)},ellipse:function(a){a=V(a);return x(a.cx||0,a.cy||0,a.rx,
a.ry)},rect:function(a){a=V(a);return s(a.x||0,a.y||0,a.width,a.height,a.rx,a.ry)},image:function(a){a=V(a);return s(a.x||0,a.y||0,a.width,a.height)},line:function(a){return"M"+[a.attr("x1")||0,a.attr("y1")||0,a.attr("x2"),a.attr("y2")]},polyline:function(a){return"M"+a.attr("points")},polygon:function(a){return"M"+a.attr("points")+"z"},deflt:function(a){a=a.node.getBBox();return s(a.x,a.y,a.width,a.height)}};a.path.toRelative=function(b){var e=A(b),h=String.prototype.toLowerCase;if(e.rel)return d(e.rel);
a.is(b,"array")&&a.is(b&&b[0],"array")||(b=a.parsePathString(b));var f=[],l=0,n=0,k=0,p=0,s=0;"M"==b[0][0]&&(l=b[0][1],n=b[0][2],k=l,p=n,s++,f.push(["M",l,n]));for(var r=b.length;s<r;s++){var q=f[s]=[],x=b[s];if(x[0]!=h.call(x[0]))switch(q[0]=h.call(x[0]),q[0]){case "a":q[1]=x[1];q[2]=x[2];q[3]=x[3];q[4]=x[4];q[5]=x[5];q[6]=+(x[6]-l).toFixed(3);q[7]=+(x[7]-n).toFixed(3);break;case "v":q[1]=+(x[1]-n).toFixed(3);break;case "m":k=x[1],p=x[2];default:for(var c=1,t=x.length;c<t;c++)q[c]=+(x[c]-(c%2?l:
n)).toFixed(3)}else for(f[s]=[],"m"==x[0]&&(k=x[1]+l,p=x[2]+n),q=0,c=x.length;q<c;q++)f[s][q]=x[q];x=f[s].length;switch(f[s][0]){case "z":l=k;n=p;break;case "h":l+=+f[s][x-1];break;case "v":n+=+f[s][x-1];break;default:l+=+f[s][x-2],n+=+f[s][x-1]}}f.toString=z;e.rel=d(f);return f};a.path.toAbsolute=G;a.path.toCubic=I;a.path.map=function(a,b){if(!b)return a;var d,e,h,f,l,n,k;a=I(a);h=0;for(l=a.length;h<l;h++)for(k=a[h],f=1,n=k.length;f<n;f+=2)d=b.x(k[f],k[f+1]),e=b.y(k[f],k[f+1]),k[f]=d,k[f+1]=e;return a};
a.path.toString=z;a.path.clone=d});C.plugin(function(a,v,y,C){var A=Math.max,w=Math.min,z=function(a){this.items=[];this.bindings={};this.length=0;this.type="set";if(a)for(var f=0,n=a.length;f<n;f++)a[f]&&(this[this.items.length]=this.items[this.items.length]=a[f],this.length++)};v=z.prototype;v.push=function(){for(var a,f,n=0,k=arguments.length;n<k;n++)if(a=arguments[n])f=this.items.length,this[f]=this.items[f]=a,this.length++;return this};v.pop=function(){this.length&&delete this[this.length--];
return this.items.pop()};v.forEach=function(a,f){for(var n=0,k=this.items.length;n<k&&!1!==a.call(f,this.items[n],n);n++);return this};v.animate=function(d,f,n,u){"function"!=typeof n||n.length||(u=n,n=L.linear);d instanceof a._.Animation&&(u=d.callback,n=d.easing,f=n.dur,d=d.attr);var p=arguments;if(a.is(d,"array")&&a.is(p[p.length-1],"array"))var b=!0;var q,e=function(){q?this.b=q:q=this.b},l=0,r=u&&function(){l++==this.length&&u.call(this)};return this.forEach(function(a,l){k.once("snap.animcreated."+
a.id,e);b?p[l]&&a.animate.apply(a,p[l]):a.animate(d,f,n,r)})};v.remove=function(){for(;this.length;)this.pop().remove();return this};v.bind=function(a,f,k){var u={};if("function"==typeof f)this.bindings[a]=f;else{var p=k||a;this.bindings[a]=function(a){u[p]=a;f.attr(u)}}return this};v.attr=function(a){var f={},k;for(k in a)if(this.bindings[k])this.bindings[k](a[k]);else f[k]=a[k];a=0;for(k=this.items.length;a<k;a++)this.items[a].attr(f);return this};v.clear=function(){for(;this.length;)this.pop()};
v.splice=function(a,f,k){a=0>a?A(this.length+a,0):a;f=A(0,w(this.length-a,f));var u=[],p=[],b=[],q;for(q=2;q<arguments.length;q++)b.push(arguments[q]);for(q=0;q<f;q++)p.push(this[a+q]);for(;q<this.length-a;q++)u.push(this[a+q]);var e=b.length;for(q=0;q<e+u.length;q++)this.items[a+q]=this[a+q]=q<e?b[q]:u[q-e];for(q=this.items.length=this.length-=f-e;this[q];)delete this[q++];return new z(p)};v.exclude=function(a){for(var f=0,k=this.length;f<k;f++)if(this[f]==a)return this.splice(f,1),!0;return!1};
v.insertAfter=function(a){for(var f=this.items.length;f--;)this.items[f].insertAfter(a);return this};v.getBBox=function(){for(var a=[],f=[],k=[],u=[],p=this.items.length;p--;)if(!this.items[p].removed){var b=this.items[p].getBBox();a.push(b.x);f.push(b.y);k.push(b.x+b.width);u.push(b.y+b.height)}a=w.apply(0,a);f=w.apply(0,f);k=A.apply(0,k);u=A.apply(0,u);return{x:a,y:f,x2:k,y2:u,width:k-a,height:u-f,cx:a+(k-a)/2,cy:f+(u-f)/2}};v.clone=function(a){a=new z;for(var f=0,k=this.items.length;f<k;f++)a.push(this.items[f].clone());
return a};v.toString=function(){return"Snap\u2018s set"};v.type="set";a.set=function(){var a=new z;arguments.length&&a.push.apply(a,Array.prototype.slice.call(arguments,0));return a}});C.plugin(function(a,v,y,C){function A(a){var b=a[0];switch(b.toLowerCase()){case "t":return[b,0,0];case "m":return[b,1,0,0,1,0,0];case "r":return 4==a.length?[b,0,a[2],a[3] ]:[b,0];case "s":return 5==a.length?[b,1,1,a[3],a[4] ]:3==a.length?[b,1,1]:[b,1]}}function w(b,d,f){d=q(d).replace(/\.{3}|\u2026/g,b);b=a.parseTransformString(b)||
[];d=a.parseTransformString(d)||[];for(var k=Math.max(b.length,d.length),p=[],v=[],h=0,w,z,y,I;h<k;h++){y=b[h]||A(d[h]);I=d[h]||A(y);if(y[0]!=I[0]||"r"==y[0].toLowerCase()&&(y[2]!=I[2]||y[3]!=I[3])||"s"==y[0].toLowerCase()&&(y[3]!=I[3]||y[4]!=I[4])){b=a._.transform2matrix(b,f());d=a._.transform2matrix(d,f());p=[["m",b.a,b.b,b.c,b.d,b.e,b.f] ];v=[["m",d.a,d.b,d.c,d.d,d.e,d.f] ];break}p[h]=[];v[h]=[];w=0;for(z=Math.max(y.length,I.length);w<z;w++)w in y&&(p[h][w]=y[w]),w in I&&(v[h][w]=I[w])}return{from:u(p),
to:u(v),f:n(p)}}function z(a){return a}function d(a){return function(b){return+b.toFixed(3)+a}}function f(b){return a.rgb(b[0],b[1],b[2])}function n(a){var b=0,d,f,k,n,h,p,q=[];d=0;for(f=a.length;d<f;d++){h="[";p=['"'+a[d][0]+'"'];k=1;for(n=a[d].length;k<n;k++)p[k]="val["+b++ +"]";h+=p+"]";q[d]=h}return Function("val","return Snap.path.toString.call(["+q+"])")}function u(a){for(var b=[],d=0,f=a.length;d<f;d++)for(var k=1,n=a[d].length;k<n;k++)b.push(a[d][k]);return b}var p={},b=/[a-z]+$/i,q=String;
p.stroke=p.fill="colour";v.prototype.equal=function(a,b){return k("snap.util.equal",this,a,b).firstDefined()};k.on("snap.util.equal",function(e,k){var r,s;r=q(this.attr(e)||"");var x=this;if(r==+r&&k==+k)return{from:+r,to:+k,f:z};if("colour"==p[e])return r=a.color(r),s=a.color(k),{from:[r.r,r.g,r.b,r.opacity],to:[s.r,s.g,s.b,s.opacity],f:f};if("transform"==e||"gradientTransform"==e||"patternTransform"==e)return k instanceof a.Matrix&&(k=k.toTransformString()),a._.rgTransform.test(k)||(k=a._.svgTransform2string(k)),
w(r,k,function(){return x.getBBox(1)});if("d"==e||"path"==e)return r=a.path.toCubic(r,k),{from:u(r[0]),to:u(r[1]),f:n(r[0])};if("points"==e)return r=q(r).split(a._.separator),s=q(k).split(a._.separator),{from:r,to:s,f:function(a){return a}};aUnit=r.match(b);s=q(k).match(b);return aUnit&&aUnit==s?{from:parseFloat(r),to:parseFloat(k),f:d(aUnit)}:{from:this.asPX(e),to:this.asPX(e,k),f:z}})});C.plugin(function(a,v,y,C){var A=v.prototype,w="createTouch"in C.doc;v="click dblclick mousedown mousemove mouseout mouseover mouseup touchstart touchmove touchend touchcancel".split(" ");
var z={mousedown:"touchstart",mousemove:"touchmove",mouseup:"touchend"},d=function(a,b){var d="y"==a?"scrollTop":"scrollLeft",e=b&&b.node?b.node.ownerDocument:C.doc;return e[d in e.documentElement?"documentElement":"body"][d]},f=function(){this.returnValue=!1},n=function(){return this.originalEvent.preventDefault()},u=function(){this.cancelBubble=!0},p=function(){return this.originalEvent.stopPropagation()},b=function(){if(C.doc.addEventListener)return function(a,b,e,f){var k=w&&z[b]?z[b]:b,l=function(k){var l=
d("y",f),q=d("x",f);if(w&&z.hasOwnProperty(b))for(var r=0,u=k.targetTouches&&k.targetTouches.length;r<u;r++)if(k.targetTouches[r].target==a||a.contains(k.targetTouches[r].target)){u=k;k=k.targetTouches[r];k.originalEvent=u;k.preventDefault=n;k.stopPropagation=p;break}return e.call(f,k,k.clientX+q,k.clientY+l)};b!==k&&a.addEventListener(b,l,!1);a.addEventListener(k,l,!1);return function(){b!==k&&a.removeEventListener(b,l,!1);a.removeEventListener(k,l,!1);return!0}};if(C.doc.attachEvent)return function(a,
b,e,h){var k=function(a){a=a||h.node.ownerDocument.window.event;var b=d("y",h),k=d("x",h),k=a.clientX+k,b=a.clientY+b;a.preventDefault=a.preventDefault||f;a.stopPropagation=a.stopPropagation||u;return e.call(h,a,k,b)};a.attachEvent("on"+b,k);return function(){a.detachEvent("on"+b,k);return!0}}}(),q=[],e=function(a){for(var b=a.clientX,e=a.clientY,f=d("y"),l=d("x"),n,p=q.length;p--;){n=q[p];if(w)for(var r=a.touches&&a.touches.length,u;r--;){if(u=a.touches[r],u.identifier==n.el._drag.id||n.el.node.contains(u.target)){b=
u.clientX;e=u.clientY;(a.originalEvent?a.originalEvent:a).preventDefault();break}}else a.preventDefault();b+=l;e+=f;k("snap.drag.move."+n.el.id,n.move_scope||n.el,b-n.el._drag.x,e-n.el._drag.y,b,e,a)}},l=function(b){a.unmousemove(e).unmouseup(l);for(var d=q.length,f;d--;)f=q[d],f.el._drag={},k("snap.drag.end."+f.el.id,f.end_scope||f.start_scope||f.move_scope||f.el,b);q=[]};for(y=v.length;y--;)(function(d){a[d]=A[d]=function(e,f){a.is(e,"function")&&(this.events=this.events||[],this.events.push({name:d,
f:e,unbind:b(this.node||document,d,e,f||this)}));return this};a["un"+d]=A["un"+d]=function(a){for(var b=this.events||[],e=b.length;e--;)if(b[e].name==d&&(b[e].f==a||!a)){b[e].unbind();b.splice(e,1);!b.length&&delete this.events;break}return this}})(v[y]);A.hover=function(a,b,d,e){return this.mouseover(a,d).mouseout(b,e||d)};A.unhover=function(a,b){return this.unmouseover(a).unmouseout(b)};var r=[];A.drag=function(b,d,f,h,n,p){function u(r,v,w){(r.originalEvent||r).preventDefault();this._drag.x=v;
this._drag.y=w;this._drag.id=r.identifier;!q.length&&a.mousemove(e).mouseup(l);q.push({el:this,move_scope:h,start_scope:n,end_scope:p});d&&k.on("snap.drag.start."+this.id,d);b&&k.on("snap.drag.move."+this.id,b);f&&k.on("snap.drag.end."+this.id,f);k("snap.drag.start."+this.id,n||h||this,v,w,r)}if(!arguments.length){var v;return this.drag(function(a,b){this.attr({transform:v+(v?"T":"t")+[a,b]})},function(){v=this.transform().local})}this._drag={};r.push({el:this,start:u});this.mousedown(u);return this};
A.undrag=function(){for(var b=r.length;b--;)r[b].el==this&&(this.unmousedown(r[b].start),r.splice(b,1),k.unbind("snap.drag.*."+this.id));!r.length&&a.unmousemove(e).unmouseup(l);return this}});C.plugin(function(a,v,y,C){y=y.prototype;var A=/^\s*url\((.+)\)/,w=String,z=a._.$;a.filter={};y.filter=function(d){var f=this;"svg"!=f.type&&(f=f.paper);d=a.parse(w(d));var k=a._.id(),u=z("filter");z(u,{id:k,filterUnits:"userSpaceOnUse"});u.appendChild(d.node);f.defs.appendChild(u);return new v(u)};k.on("snap.util.getattr.filter",
function(){k.stop();var d=z(this.node,"filter");if(d)return(d=w(d).match(A))&&a.select(d[1])});k.on("snap.util.attr.filter",function(d){if(d instanceof v&&"filter"==d.type){k.stop();var f=d.node.id;f||(z(d.node,{id:d.id}),f=d.id);z(this.node,{filter:a.url(f)})}d&&"none"!=d||(k.stop(),this.node.removeAttribute("filter"))});a.filter.blur=function(d,f){null==d&&(d=2);return a.format('<feGaussianBlur stdDeviation="{def}"/>',{def:null==f?d:[d,f]})};a.filter.blur.toString=function(){return this()};a.filter.shadow=
function(d,f,k,u,p){"string"==typeof k&&(p=u=k,k=4);"string"!=typeof u&&(p=u,u="#000");null==k&&(k=4);null==p&&(p=1);null==d&&(d=0,f=2);null==f&&(f=d);u=a.color(u||"#000");return a.format('<feGaussianBlur in="SourceAlpha" stdDeviation="{blur}"/><feOffset dx="{dx}" dy="{dy}" result="offsetblur"/><feFlood flood-color="{color}"/><feComposite in2="offsetblur" operator="in"/><feComponentTransfer><feFuncA type="linear" slope="{opacity}"/></feComponentTransfer><feMerge><feMergeNode/><feMergeNode in="SourceGraphic"/></feMerge>',
{color:u,dx:d,dy:f,blur:k,opacity:p})};a.filter.shadow.toString=function(){return this()};a.filter.grayscale=function(d){null==d&&(d=1);return a.format('<feColorMatrix type="matrix" values="{a} {b} {c} 0 0 {d} {e} {f} 0 0 {g} {b} {h} 0 0 0 0 0 1 0"/>',{a:0.2126+0.7874*(1-d),b:0.7152-0.7152*(1-d),c:0.0722-0.0722*(1-d),d:0.2126-0.2126*(1-d),e:0.7152+0.2848*(1-d),f:0.0722-0.0722*(1-d),g:0.2126-0.2126*(1-d),h:0.0722+0.9278*(1-d)})};a.filter.grayscale.toString=function(){return this()};a.filter.sepia=
function(d){null==d&&(d=1);return a.format('<feColorMatrix type="matrix" values="{a} {b} {c} 0 0 {d} {e} {f} 0 0 {g} {h} {i} 0 0 0 0 0 1 0"/>',{a:0.393+0.607*(1-d),b:0.769-0.769*(1-d),c:0.189-0.189*(1-d),d:0.349-0.349*(1-d),e:0.686+0.314*(1-d),f:0.168-0.168*(1-d),g:0.272-0.272*(1-d),h:0.534-0.534*(1-d),i:0.131+0.869*(1-d)})};a.filter.sepia.toString=function(){return this()};a.filter.saturate=function(d){null==d&&(d=1);return a.format('<feColorMatrix type="saturate" values="{amount}"/>',{amount:1-
d})};a.filter.saturate.toString=function(){return this()};a.filter.hueRotate=function(d){return a.format('<feColorMatrix type="hueRotate" values="{angle}"/>',{angle:d||0})};a.filter.hueRotate.toString=function(){return this()};a.filter.invert=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="table" tableValues="{amount} {amount2}"/><feFuncG type="table" tableValues="{amount} {amount2}"/><feFuncB type="table" tableValues="{amount} {amount2}"/></feComponentTransfer>',{amount:d,
amount2:1-d})};a.filter.invert.toString=function(){return this()};a.filter.brightness=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="linear" slope="{amount}"/><feFuncG type="linear" slope="{amount}"/><feFuncB type="linear" slope="{amount}"/></feComponentTransfer>',{amount:d})};a.filter.brightness.toString=function(){return this()};a.filter.contrast=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="linear" slope="{amount}" intercept="{amount2}"/><feFuncG type="linear" slope="{amount}" intercept="{amount2}"/><feFuncB type="linear" slope="{amount}" intercept="{amount2}"/></feComponentTransfer>',
{amount:d,amount2:0.5-d/2})};a.filter.contrast.toString=function(){return this()}});return C});

]]> </script>
<script> <![CDATA[

(function (glob, factory) {
    // AMD support
    if (typeof define === "function" && define.amd) {
        // Define as an anonymous module
        define("Gadfly", ["Snap.svg"], function (Snap) {
            return factory(Snap);
        });
    } else {
        // Browser globals (glob is window)
        // Snap adds itself to window
        glob.Gadfly = factory(glob.Snap);
    }
}(this, function (Snap) {

var Gadfly = {};

// Get an x/y coordinate value in pixels
var xPX = function(fig, x) {
    var client_box = fig.node.getBoundingClientRect();
    return x * fig.node.viewBox.baseVal.width / client_box.width;
};

var yPX = function(fig, y) {
    var client_box = fig.node.getBoundingClientRect();
    return y * fig.node.viewBox.baseVal.height / client_box.height;
};


Snap.plugin(function (Snap, Element, Paper, global) {
    // Traverse upwards from a snap element to find and return the first
    // note with the "plotroot" class.
    Element.prototype.plotroot = function () {
        var element = this;
        while (!element.hasClass("plotroot") && element.parent() != null) {
            element = element.parent();
        }
        return element;
    };

    Element.prototype.svgroot = function () {
        var element = this;
        while (element.node.nodeName != "svg" && element.parent() != null) {
            element = element.parent();
        }
        return element;
    };

    Element.prototype.plotbounds = function () {
        var root = this.plotroot()
        var bbox = root.select(".guide.background").node.getBBox();
        return {
            x0: bbox.x,
            x1: bbox.x + bbox.width,
            y0: bbox.y,
            y1: bbox.y + bbox.height
        };
    };

    Element.prototype.plotcenter = function () {
        var root = this.plotroot()
        var bbox = root.select(".guide.background").node.getBBox();
        return {
            x: bbox.x + bbox.width / 2,
            y: bbox.y + bbox.height / 2
        };
    };

    // Emulate IE style mouseenter/mouseleave events, since Microsoft always
    // does everything right.
    // See: http://www.dynamic-tools.net/toolbox/isMouseLeaveOrEnter/
    var events = ["mouseenter", "mouseleave"];

    for (i in events) {
        (function (event_name) {
            var event_name = events[i];
            Element.prototype[event_name] = function (fn, scope) {
                if (Snap.is(fn, "function")) {
                    var fn2 = function (event) {
                        if (event.type != "mouseover" && event.type != "mouseout") {
                            return;
                        }

                        var reltg = event.relatedTarget ? event.relatedTarget :
                            event.type == "mouseout" ? event.toElement : event.fromElement;
                        while (reltg && reltg != this.node) reltg = reltg.parentNode;

                        if (reltg != this.node) {
                            return fn.apply(this, event);
                        }
                    };

                    if (event_name == "mouseenter") {
                        this.mouseover(fn2, scope);
                    } else {
                        this.mouseout(fn2, scope);
                    }
                }
                return this;
            };
        })(events[i]);
    }


    Element.prototype.mousewheel = function (fn, scope) {
        if (Snap.is(fn, "function")) {
            var el = this;
            var fn2 = function (event) {
                fn.apply(el, [event]);
            };
        }

        this.node.addEventListener(
            /Firefox/i.test(navigator.userAgent) ? "DOMMouseScroll" : "mousewheel",
            fn2);

        return this;
    };


    // Snap's attr function can be too slow for things like panning/zooming.
    // This is a function to directly update element attributes without going
    // through eve.
    Element.prototype.attribute = function(key, val) {
        if (val === undefined) {
            return this.node.getAttribute(key);
        } else {
            this.node.setAttribute(key, val);
            return this;
        }
    };

    Element.prototype.init_gadfly = function() {
        this.mouseenter(Gadfly.plot_mouseover)
            .mouseleave(Gadfly.plot_mouseout)
            .dblclick(Gadfly.plot_dblclick)
            .mousewheel(Gadfly.guide_background_scroll)
            .drag(Gadfly.guide_background_drag_onmove,
                  Gadfly.guide_background_drag_onstart,
                  Gadfly.guide_background_drag_onend);
        this.mouseenter(function (event) {
            init_pan_zoom(this.plotroot());
        });
        return this;
    };
});


// When the plot is moused over, emphasize the grid lines.
Gadfly.plot_mouseover = function(event) {
    var root = this.plotroot();

    var keyboard_zoom = function(event) {
        if (event.which == 187) { // plus
            increase_zoom_by_position(root, 0.1, true);
        } else if (event.which == 189) { // minus
            increase_zoom_by_position(root, -0.1, true);
        }
    };
    root.data("keyboard_zoom", keyboard_zoom);
    window.addEventListener("keyup", keyboard_zoom);

    var xgridlines = root.select(".xgridlines"),
        ygridlines = root.select(".ygridlines");

    xgridlines.data("unfocused_strokedash",
                    xgridlines.attribute("stroke-dasharray").replace(/(\d)(,|$)/g, "$1mm$2"));
    ygridlines.data("unfocused_strokedash",
                    ygridlines.attribute("stroke-dasharray").replace(/(\d)(,|$)/g, "$1mm$2"));

    // emphasize grid lines
    var destcolor = root.data("focused_xgrid_color");
    xgridlines.attribute("stroke-dasharray", "none")
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    destcolor = root.data("focused_ygrid_color");
    ygridlines.attribute("stroke-dasharray", "none")
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    // reveal zoom slider
    root.select(".zoomslider")
        .animate({opacity: 1.0}, 250);
};

// Reset pan and zoom on double click
Gadfly.plot_dblclick = function(event) {
  set_plot_pan_zoom(this.plotroot(), 0.0, 0.0, 1.0);
};

// Unemphasize grid lines on mouse out.
Gadfly.plot_mouseout = function(event) {
    var root = this.plotroot();

    window.removeEventListener("keyup", root.data("keyboard_zoom"));
    root.data("keyboard_zoom", undefined);

    var xgridlines = root.select(".xgridlines"),
        ygridlines = root.select(".ygridlines");

    var destcolor = root.data("unfocused_xgrid_color");

    xgridlines.attribute("stroke-dasharray", xgridlines.data("unfocused_strokedash"))
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    destcolor = root.data("unfocused_ygrid_color");
    ygridlines.attribute("stroke-dasharray", ygridlines.data("unfocused_strokedash"))
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    // hide zoom slider
    root.select(".zoomslider")
        .animate({opacity: 0.0}, 250);
};


var set_geometry_transform = function(root, tx, ty, scale) {
    var xscalable = root.hasClass("xscalable"),
        yscalable = root.hasClass("yscalable");

    var old_scale = root.data("scale");

    var xscale = xscalable ? scale : 1.0,
        yscale = yscalable ? scale : 1.0;

    tx = xscalable ? tx : 0.0;
    ty = yscalable ? ty : 0.0;

    var t = new Snap.Matrix().translate(tx, ty).scale(xscale, yscale);

    root.selectAll(".geometry, image")
        .forEach(function (element, i) {
            element.transform(t);
        });

    bounds = root.plotbounds();

    if (yscalable) {
        var xfixed_t = new Snap.Matrix().translate(0, ty).scale(1.0, yscale);
        root.selectAll(".xfixed")
            .forEach(function (element, i) {
                element.transform(xfixed_t);
            });

        root.select(".ylabels")
            .transform(xfixed_t)
            .selectAll("text")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var cx = element.asPX("x"),
                        cy = element.asPX("y");
                    var st = element.data("static_transform");
                    unscale_t = new Snap.Matrix();
                    unscale_t.scale(1, 1/scale, cx, cy).add(st);
                    element.transform(unscale_t);

                    var y = cy * scale + ty;
                    element.attr("visibility",
                        bounds.y0 <= y && y <= bounds.y1 ? "visible" : "hidden");
                }
            });
    }

    if (xscalable) {
        var yfixed_t = new Snap.Matrix().translate(tx, 0).scale(xscale, 1.0);
        var xtrans = new Snap.Matrix().translate(tx, 0);
        root.selectAll(".yfixed")
            .forEach(function (element, i) {
                element.transform(yfixed_t);
            });

        root.select(".xlabels")
            .transform(yfixed_t)
            .selectAll("text")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var cx = element.asPX("x"),
                        cy = element.asPX("y");
                    var st = element.data("static_transform");
                    unscale_t = new Snap.Matrix();
                    unscale_t.scale(1/scale, 1, cx, cy).add(st);

                    element.transform(unscale_t);

                    var x = cx * scale + tx;
                    element.attr("visibility",
                        bounds.x0 <= x && x <= bounds.x1 ? "visible" : "hidden");
                    }
            });
    }

    // we must unscale anything that is scale invariance: widths, raiduses, etc.
    var size_attribs = ["font-size"];
    var unscaled_selection = ".geometry, .geometry *";
    if (xscalable) {
        size_attribs.push("rx");
        unscaled_selection += ", .xgridlines";
    }
    if (yscalable) {
        size_attribs.push("ry");
        unscaled_selection += ", .ygridlines";
    }

    root.selectAll(unscaled_selection)
        .forEach(function (element, i) {
            // circle need special help
            if (element.node.nodeName == "circle") {
                var cx = element.attribute("cx"),
                    cy = element.attribute("cy");
                unscale_t = new Snap.Matrix().scale(1/xscale, 1/yscale,
                                                        cx, cy);
                element.transform(unscale_t);
                return;
            }

            for (i in size_attribs) {
                var key = size_attribs[i];
                var val = parseFloat(element.attribute(key));
                if (val !== undefined && val != 0 && !isNaN(val)) {
                    element.attribute(key, val * old_scale / scale);
                }
            }
        });
};


// Find the most appropriate tick scale and update label visibility.
var update_tickscale = function(root, scale, axis) {
    if (!root.hasClass(axis + "scalable")) return;

    var tickscales = root.data(axis + "tickscales");
    var best_tickscale = 1.0;
    var best_tickscale_dist = Infinity;
    for (tickscale in tickscales) {
        var dist = Math.abs(Math.log(tickscale) - Math.log(scale));
        if (dist < best_tickscale_dist) {
            best_tickscale_dist = dist;
            best_tickscale = tickscale;
        }
    }

    if (best_tickscale != root.data(axis + "tickscale")) {
        root.data(axis + "tickscale", best_tickscale);
        var mark_inscale_gridlines = function (element, i) {
            var inscale = element.attr("gadfly:scale") == best_tickscale;
            element.attribute("gadfly:inscale", inscale);
            element.attr("visibility", inscale ? "visible" : "hidden");
        };

        var mark_inscale_labels = function (element, i) {
            var inscale = element.attr("gadfly:scale") == best_tickscale;
            element.attribute("gadfly:inscale", inscale);
            element.attr("visibility", inscale ? "visible" : "hidden");
        };

        root.select("." + axis + "gridlines").selectAll("path").forEach(mark_inscale_gridlines);
        root.select("." + axis + "labels").selectAll("text").forEach(mark_inscale_labels);
    }
};


var set_plot_pan_zoom = function(root, tx, ty, scale) {
    var old_scale = root.data("scale");
    var bounds = root.plotbounds();

    var width = bounds.x1 - bounds.x0,
        height = bounds.y1 - bounds.y0;

    // compute the viewport derived from tx, ty, and scale
    var x_min = -width * scale - (scale * width - width),
        x_max = width * scale,
        y_min = -height * scale - (scale * height - height),
        y_max = height * scale;

    var x0 = bounds.x0 - scale * bounds.x0,
        y0 = bounds.y0 - scale * bounds.y0;

    var tx = Math.max(Math.min(tx - x0, x_max), x_min),
        ty = Math.max(Math.min(ty - y0, y_max), y_min);

    tx += x0;
    ty += y0;

    // when the scale change, we may need to alter which set of
    // ticks is being displayed
    if (scale != old_scale) {
        update_tickscale(root, scale, "x");
        update_tickscale(root, scale, "y");
    }

    set_geometry_transform(root, tx, ty, scale);

    root.data("scale", scale);
    root.data("tx", tx);
    root.data("ty", ty);
};


var scale_centered_translation = function(root, scale) {
    var bounds = root.plotbounds();

    var width = bounds.x1 - bounds.x0,
        height = bounds.y1 - bounds.y0;

    var tx0 = root.data("tx"),
        ty0 = root.data("ty");

    var scale0 = root.data("scale");

    // how off from center the current view is
    var xoff = tx0 - (bounds.x0 * (1 - scale0) + (width * (1 - scale0)) / 2),
        yoff = ty0 - (bounds.y0 * (1 - scale0) + (height * (1 - scale0)) / 2);

    // rescale offsets
    xoff = xoff * scale / scale0;
    yoff = yoff * scale / scale0;

    // adjust for the panel position being scaled
    var x_edge_adjust = bounds.x0 * (1 - scale),
        y_edge_adjust = bounds.y0 * (1 - scale);

    return {
        x: xoff + x_edge_adjust + (width - width * scale) / 2,
        y: yoff + y_edge_adjust + (height - height * scale) / 2
    };
};


// Initialize data for panning zooming if it isn't already.
var init_pan_zoom = function(root) {
    if (root.data("zoompan-ready")) {
        return;
    }

    // The non-scaling-stroke trick. Rather than try to correct for the
    // stroke-width when zooming, we force it to a fixed value.
    var px_per_mm = root.node.getCTM().a;

    // Drag events report deltas in pixels, which we'd like to convert to
    // millimeters.
    root.data("px_per_mm", px_per_mm);

    root.selectAll("path")
        .forEach(function (element, i) {
        sw = element.asPX("stroke-width") * px_per_mm;
        if (sw > 0) {
            element.attribute("stroke-width", sw);
            element.attribute("vector-effect", "non-scaling-stroke");
        }
    });

    // Store ticks labels original tranformation
    root.selectAll(".xlabels > text, .ylabels > text")
        .forEach(function (element, i) {
            var lm = element.transform().localMatrix;
            element.data("static_transform",
                new Snap.Matrix(lm.a, lm.b, lm.c, lm.d, lm.e, lm.f));
        });

    var xgridlines = root.select(".xgridlines");
    var ygridlines = root.select(".ygridlines");
    var xlabels = root.select(".xlabels");
    var ylabels = root.select(".ylabels");

    if (root.data("tx") === undefined) root.data("tx", 0);
    if (root.data("ty") === undefined) root.data("ty", 0);
    if (root.data("scale") === undefined) root.data("scale", 1.0);
    if (root.data("xtickscales") === undefined) {

        // index all the tick scales that are listed
        var xtickscales = {};
        var ytickscales = {};
        var add_x_tick_scales = function (element, i) {
            xtickscales[element.attribute("gadfly:scale")] = true;
        };
        var add_y_tick_scales = function (element, i) {
            ytickscales[element.attribute("gadfly:scale")] = true;
        };

        if (xgridlines) xgridlines.selectAll("path").forEach(add_x_tick_scales);
        if (ygridlines) ygridlines.selectAll("path").forEach(add_y_tick_scales);
        if (xlabels) xlabels.selectAll("text").forEach(add_x_tick_scales);
        if (ylabels) ylabels.selectAll("text").forEach(add_y_tick_scales);

        root.data("xtickscales", xtickscales);
        root.data("ytickscales", ytickscales);
        root.data("xtickscale", 1.0);
    }

    var min_scale = 1.0, max_scale = 1.0;
    for (scale in xtickscales) {
        min_scale = Math.min(min_scale, scale);
        max_scale = Math.max(max_scale, scale);
    }
    for (scale in ytickscales) {
        min_scale = Math.min(min_scale, scale);
        max_scale = Math.max(max_scale, scale);
    }
    root.data("min_scale", min_scale);
    root.data("max_scale", max_scale);

    // store the original positions of labels
    if (xlabels) {
        xlabels.selectAll("text")
               .forEach(function (element, i) {
                   element.data("x", element.asPX("x"));
               });
    }

    if (ylabels) {
        ylabels.selectAll("text")
               .forEach(function (element, i) {
                   element.data("y", element.asPX("y"));
               });
    }

    // mark grid lines and ticks as in or out of scale.
    var mark_inscale = function (element, i) {
        element.attribute("gadfly:inscale", element.attribute("gadfly:scale") == 1.0);
    };

    if (xgridlines) xgridlines.selectAll("path").forEach(mark_inscale);
    if (ygridlines) ygridlines.selectAll("path").forEach(mark_inscale);
    if (xlabels) xlabels.selectAll("text").forEach(mark_inscale);
    if (ylabels) ylabels.selectAll("text").forEach(mark_inscale);

    // figure out the upper ond lower bounds on panning using the maximum
    // and minum grid lines
    var bounds = root.plotbounds();
    var pan_bounds = {
        x0: 0.0,
        y0: 0.0,
        x1: 0.0,
        y1: 0.0
    };

    if (xgridlines) {
        xgridlines
            .selectAll("path")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var bbox = element.node.getBBox();
                    if (bounds.x1 - bbox.x < pan_bounds.x0) {
                        pan_bounds.x0 = bounds.x1 - bbox.x;
                    }
                    if (bounds.x0 - bbox.x > pan_bounds.x1) {
                        pan_bounds.x1 = bounds.x0 - bbox.x;
                    }
                    element.attr("visibility", "visible");
                }
            });
    }

    if (ygridlines) {
        ygridlines
            .selectAll("path")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var bbox = element.node.getBBox();
                    if (bounds.y1 - bbox.y < pan_bounds.y0) {
                        pan_bounds.y0 = bounds.y1 - bbox.y;
                    }
                    if (bounds.y0 - bbox.y > pan_bounds.y1) {
                        pan_bounds.y1 = bounds.y0 - bbox.y;
                    }
                    element.attr("visibility", "visible");
                }
            });
    }

    // nudge these values a little
    pan_bounds.x0 -= 5;
    pan_bounds.x1 += 5;
    pan_bounds.y0 -= 5;
    pan_bounds.y1 += 5;
    root.data("pan_bounds", pan_bounds);

    root.data("zoompan-ready", true)
};


// drag actions, i.e. zooming and panning
var pan_action = {
    start: function(root, x, y, event) {
        root.data("dx", 0);
        root.data("dy", 0);
        root.data("tx0", root.data("tx"));
        root.data("ty0", root.data("ty"));
    },
    update: function(root, dx, dy, x, y, event) {
        var px_per_mm = root.data("px_per_mm");
        dx /= px_per_mm;
        dy /= px_per_mm;

        var tx0 = root.data("tx"),
            ty0 = root.data("ty");

        var dx0 = root.data("dx"),
            dy0 = root.data("dy");

        root.data("dx", dx);
        root.data("dy", dy);

        dx = dx - dx0;
        dy = dy - dy0;

        var tx = tx0 + dx,
            ty = ty0 + dy;

        set_plot_pan_zoom(root, tx, ty, root.data("scale"));
    },
    end: function(root, event) {

    },
    cancel: function(root) {
        set_plot_pan_zoom(root, root.data("tx0"), root.data("ty0"), root.data("scale"));
    }
};

var zoom_box;
var zoom_action = {
    start: function(root, x, y, event) {
        var bounds = root.plotbounds();
        var width = bounds.x1 - bounds.x0,
            height = bounds.y1 - bounds.y0;
        var ratio = width / height;
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var px_per_mm = root.data("px_per_mm");
        x = xscalable ? x / px_per_mm : bounds.x0;
        y = yscalable ? y / px_per_mm : bounds.y0;
        var w = xscalable ? 0 : width;
        var h = yscalable ? 0 : height;
        zoom_box = root.rect(x, y, w, h).attr({
            "fill": "#000",
            "opacity": 0.25
        });
        zoom_box.data("ratio", ratio);
    },
    update: function(root, dx, dy, x, y, event) {
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var px_per_mm = root.data("px_per_mm");
        var bounds = root.plotbounds();
        if (yscalable) {
            y /= px_per_mm;
            y = Math.max(bounds.y0, y);
            y = Math.min(bounds.y1, y);
        } else {
            y = bounds.y1;
        }
        if (xscalable) {
            x /= px_per_mm;
            x = Math.max(bounds.x0, x);
            x = Math.min(bounds.x1, x);
        } else {
            x = bounds.x1;
        }

        dx = x - zoom_box.attr("x");
        dy = y - zoom_box.attr("y");
        if (xscalable && yscalable) {
            var ratio = zoom_box.data("ratio");
            var width = Math.min(Math.abs(dx), ratio * Math.abs(dy));
            var height = Math.min(Math.abs(dy), Math.abs(dx) / ratio);
            dx = width * dx / Math.abs(dx);
            dy = height * dy / Math.abs(dy);
        }
        var xoffset = 0,
            yoffset = 0;
        if (dx < 0) {
            xoffset = dx;
            dx = -1 * dx;
        }
        if (dy < 0) {
            yoffset = dy;
            dy = -1 * dy;
        }
        if (isNaN(dy)) {
            dy = 0.0;
        }
        if (isNaN(dx)) {
            dx = 0.0;
        }
        zoom_box.transform("T" + xoffset + "," + yoffset);
        zoom_box.attr("width", dx);
        zoom_box.attr("height", dy);
    },
    end: function(root, event) {
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var zoom_bounds = zoom_box.getBBox();
        if (zoom_bounds.width * zoom_bounds.height <= 0) {
            return;
        }
        var plot_bounds = root.plotbounds();
        var zoom_factor = 1.0;
        if (yscalable) {
            zoom_factor = (plot_bounds.y1 - plot_bounds.y0) / zoom_bounds.height;
        } else {
            zoom_factor = (plot_bounds.x1 - plot_bounds.x0) / zoom_bounds.width;
        }
        var tx = (root.data("tx") - zoom_bounds.x) * zoom_factor + plot_bounds.x0,
            ty = (root.data("ty") - zoom_bounds.y) * zoom_factor + plot_bounds.y0;
        set_plot_pan_zoom(root, tx, ty, root.data("scale") * zoom_factor);
        zoom_box.remove();
    },
    cancel: function(root) {
        zoom_box.remove();
    }
};


Gadfly.guide_background_drag_onstart = function(x, y, event) {
    var root = this.plotroot();
    var scalable = root.hasClass("xscalable") || root.hasClass("yscalable");
    var zoomable = !event.altKey && !event.ctrlKey && event.shiftKey && scalable;
    var panable = !event.altKey && !event.ctrlKey && !event.shiftKey && scalable;
    var drag_action = zoomable ? zoom_action :
                      panable  ? pan_action :
                                 undefined;
    root.data("drag_action", drag_action);
    if (drag_action) {
        var cancel_drag_action = function(event) {
            if (event.which == 27) { // esc key
                drag_action.cancel(root);
                root.data("drag_action", undefined);
            }
        };
        window.addEventListener("keyup", cancel_drag_action);
        root.data("cancel_drag_action", cancel_drag_action);
        drag_action.start(root, x, y, event);
    }
};


Gadfly.guide_background_drag_onmove = function(dx, dy, x, y, event) {
    var root = this.plotroot();
    var drag_action = root.data("drag_action");
    if (drag_action) {
        drag_action.update(root, dx, dy, x, y, event);
    }
};


Gadfly.guide_background_drag_onend = function(event) {
    var root = this.plotroot();
    window.removeEventListener("keyup", root.data("cancel_drag_action"));
    root.data("cancel_drag_action", undefined);
    var drag_action = root.data("drag_action");
    if (drag_action) {
        drag_action.end(root, event);
    }
    root.data("drag_action", undefined);
};


Gadfly.guide_background_scroll = function(event) {
    if (event.shiftKey) {
        increase_zoom_by_position(this.plotroot(), 0.001 * event.wheelDelta);
        event.preventDefault();
    }
};


Gadfly.zoomslider_button_mouseover = function(event) {
    this.select(".button_logo")
         .animate({fill: this.data("mouseover_color")}, 100);
};


Gadfly.zoomslider_button_mouseout = function(event) {
     this.select(".button_logo")
         .animate({fill: this.data("mouseout_color")}, 100);
};


Gadfly.zoomslider_zoomout_click = function(event) {
    increase_zoom_by_position(this.plotroot(), -0.1, true);
};


Gadfly.zoomslider_zoomin_click = function(event) {
    increase_zoom_by_position(this.plotroot(), 0.1, true);
};


Gadfly.zoomslider_track_click = function(event) {
    // TODO
};


// Map slider position x to scale y using the function y = a*exp(b*x)+c.
// The constants a, b, and c are solved using the constraint that the function
// should go through the points (0; min_scale), (0.5; 1), and (1; max_scale).
var scale_from_slider_position = function(position, min_scale, max_scale) {
    var a = (1 - 2 * min_scale + min_scale * min_scale) / (min_scale + max_scale - 2),
        b = 2 * Math.log((max_scale - 1) / (1 - min_scale)),
        c = (min_scale * max_scale - 1) / (min_scale + max_scale - 2);
    return a * Math.exp(b * position) + c;
}

// inverse of scale_from_slider_position
var slider_position_from_scale = function(scale, min_scale, max_scale) {
    var a = (1 - 2 * min_scale + min_scale * min_scale) / (min_scale + max_scale - 2),
        b = 2 * Math.log((max_scale - 1) / (1 - min_scale)),
        c = (min_scale * max_scale - 1) / (min_scale + max_scale - 2);
    return 1 / b * Math.log((scale - c) / a);
}

var increase_zoom_by_position = function(root, delta_position, animate) {
    var scale = root.data("scale"),
        min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale");
    var position = slider_position_from_scale(scale, min_scale, max_scale);
    position += delta_position;
    scale = scale_from_slider_position(position, min_scale, max_scale);
    set_zoom(root, scale, animate);
}

var set_zoom = function(root, scale, animate) {
    var min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale"),
        old_scale = root.data("scale");
    var new_scale = Math.max(min_scale, Math.min(scale, max_scale));
    if (animate) {
        Snap.animate(
            old_scale,
            new_scale,
            function (new_scale) {
                update_plot_scale(root, new_scale);
            },
            200);
    } else {
        update_plot_scale(root, new_scale);
    }
}


var update_plot_scale = function(root, new_scale) {
    var trans = scale_centered_translation(root, new_scale);
    set_plot_pan_zoom(root, trans.x, trans.y, new_scale);

    root.selectAll(".zoomslider_thumb")
        .forEach(function (element, i) {
            var min_pos = element.data("min_pos"),
                max_pos = element.data("max_pos"),
                min_scale = root.data("min_scale"),
                max_scale = root.data("max_scale");
            var xmid = (min_pos + max_pos) / 2;
            var xpos = slider_position_from_scale(new_scale, min_scale, max_scale);
            element.transform(new Snap.Matrix().translate(
                Math.max(min_pos, Math.min(
                         max_pos, min_pos + (max_pos - min_pos) * xpos)) - xmid, 0));
    });
};


Gadfly.zoomslider_thumb_dragmove = function(dx, dy, x, y, event) {
    var root = this.plotroot();
    var min_pos = this.data("min_pos"),
        max_pos = this.data("max_pos"),
        min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale"),
        old_scale = root.data("old_scale");

    var px_per_mm = root.data("px_per_mm");
    dx /= px_per_mm;
    dy /= px_per_mm;

    var xmid = (min_pos + max_pos) / 2;
    var xpos = slider_position_from_scale(old_scale, min_scale, max_scale) +
                   dx / (max_pos - min_pos);

    // compute the new scale
    var new_scale = scale_from_slider_position(xpos, min_scale, max_scale);
    new_scale = Math.min(max_scale, Math.max(min_scale, new_scale));

    update_plot_scale(root, new_scale);
    event.stopPropagation();
};


Gadfly.zoomslider_thumb_dragstart = function(x, y, event) {
    this.animate({fill: this.data("mouseover_color")}, 100);
    var root = this.plotroot();

    // keep track of what the scale was when we started dragging
    root.data("old_scale", root.data("scale"));
    event.stopPropagation();
};


Gadfly.zoomslider_thumb_dragend = function(event) {
    this.animate({fill: this.data("mouseout_color")}, 100);
    event.stopPropagation();
};


var toggle_color_class = function(root, color_class, ison) {
    var guides = root.selectAll(".guide." + color_class + ",.guide ." + color_class);
    var geoms = root.selectAll(".geometry." + color_class + ",.geometry ." + color_class);
    if (ison) {
        guides.animate({opacity: 0.5}, 250);
        geoms.animate({opacity: 0.0}, 250);
    } else {
        guides.animate({opacity: 1.0}, 250);
        geoms.animate({opacity: 1.0}, 250);
    }
};


Gadfly.colorkey_swatch_click = function(event) {
    var root = this.plotroot();
    var color_class = this.data("color_class");

    if (event.shiftKey) {
        root.selectAll(".colorkey text")
            .forEach(function (element) {
                var other_color_class = element.data("color_class");
                if (other_color_class != color_class) {
                    toggle_color_class(root, other_color_class,
                                       element.attr("opacity") == 1.0);
                }
            });
    } else {
        toggle_color_class(root, color_class, this.attr("opacity") == 1.0);
    }
};


return Gadfly;

}));


//@ sourceURL=gadfly.js

(function (glob, factory) {
    // AMD support
      if (typeof require === "function" && typeof define === "function" && define.amd) {
        require(["Snap.svg", "Gadfly"], function (Snap, Gadfly) {
            factory(Snap, Gadfly);
        });
      } else {
          factory(glob.Snap, glob.Gadfly);
      }
})(window, function (Snap, Gadfly) {
    var fig = Snap("#img-9020955a");
fig.select("#img-9020955a-4")
   .drag(function() {}, function() {}, function() {});
fig.select("#img-9020955a-6")
   .data("color_class", "color_Emission")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-9020955a-7")
   .data("color_class", "color_SoilMoisture")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-9020955a-8")
   .data("color_class", "color_t2m")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-9020955a-10")
   .data("color_class", "color_Emission")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-9020955a-11")
   .data("color_class", "color_SoilMoisture")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-9020955a-12")
   .data("color_class", "color_t2m")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-9020955a-16")
   .init_gadfly();
fig.select("#img-9020955a-19")
   .plotroot().data("unfocused_ygrid_color", "#D0D0E0")
;
fig.select("#img-9020955a-19")
   .plotroot().data("focused_ygrid_color", "#A0A0A0")
;
fig.select("#img-9020955a-173")
   .plotroot().data("unfocused_xgrid_color", "#D0D0E0")
;
fig.select("#img-9020955a-173")
   .plotroot().data("focused_xgrid_color", "#A0A0A0")
;
fig.select("#img-9020955a-212")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-9020955a-212")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-9020955a-212")
   .click(Gadfly.zoomslider_zoomin_click)
.mouseenter(Gadfly.zoomslider_button_mouseover)
.mouseleave(Gadfly.zoomslider_button_mouseout)
;
fig.select("#img-9020955a-216")
   .data("max_pos", 101.45)
;
fig.select("#img-9020955a-216")
   .data("min_pos", 84.45)
;
fig.select("#img-9020955a-216")
   .click(Gadfly.zoomslider_track_click);
fig.select("#img-9020955a-218")
   .data("max_pos", 101.45)
;
fig.select("#img-9020955a-218")
   .data("min_pos", 84.45)
;
fig.select("#img-9020955a-218")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-9020955a-218")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-9020955a-218")
   .drag(Gadfly.zoomslider_thumb_dragmove,
     Gadfly.zoomslider_thumb_dragstart,
     Gadfly.zoomslider_thumb_dragend)
;
fig.select("#img-9020955a-220")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-9020955a-220")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-9020955a-220")
   .click(Gadfly.zoomslider_zoomout_click)
.mouseenter(Gadfly.zoomslider_button_mouseover)
.mouseleave(Gadfly.zoomslider_button_mouseout)
;
    });
]]> </script>
</svg>




```julia
plotTS(cube_normalized)
```






















<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     xmlns:gadfly="http://www.gadflyjl.org/ns"
     version="1.2"
     width="141.42mm" height="100mm" viewBox="0 0 141.42 100"
     stroke="none"
     fill="#000000"
     stroke-width="0.3"
     font-size="3.88"

     id="img-91c8bd3d">
<g class="plotroot xscalable yscalable" id="img-91c8bd3d-1">
  <g font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" fill="#564A55" stroke="#000000" stroke-opacity="0.000" id="img-91c8bd3d-2">
    <text x="67.36" y="88.39" text-anchor="middle" dy="0.6em">x</text>
  </g>
  <g class="guide xlabels" font-size="2.82" font-family="'PT Sans Caption','Helvetica Neue','Helvetica',sans-serif" fill="#6C606B" id="img-91c8bd3d-3">
    <text x="-108.98" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">Jan 1, 1980</text>
    <text x="-76.91" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1985</text>
    <text x="-44.85" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1990</text>
    <text x="-12.79" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1995</text>
    <text x="19.27" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2000</text>
    <text x="51.34" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2005</text>
    <text x="83.4" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2010</text>
    <text x="115.45" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2015</text>
    <text x="147.51" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2020</text>
    <text x="179.59" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2025</text>
    <text x="211.64" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2030</text>
    <text x="243.7" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2035</text>
    <text x="-108.98" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">Jan 1, 1980</text>
    <text x="-44.85" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">1990</text>
    <text x="19.27" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2000</text>
    <text x="83.4" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2010</text>
    <text x="147.51" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2020</text>
    <text x="211.64" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2030</text>
    <text x="-108.98" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">Jan 1, 1980</text>
    <text x="-44.85" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">1990</text>
    <text x="19.27" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2000</text>
    <text x="83.4" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2010</text>
    <text x="147.51" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2020</text>
    <text x="211.64" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2030</text>
    <text x="-108.98" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">Jan 1, 1980</text>
    <text x="-44.85" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">1990</text>
    <text x="19.27" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2000</text>
    <text x="83.4" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2010</text>
    <text x="147.51" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2020</text>
    <text x="211.64" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2030</text>
  </g>
  <g class="guide colorkey" id="img-91c8bd3d-4">
    <g fill="#4C404B" font-size="2.82" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" id="img-91c8bd3d-5">
      <text x="121.27" y="41.04" dy="0.35em" id="img-91c8bd3d-6" class="color_Emission">Emission</text>
      <text x="121.27" y="44.67" dy="0.35em" id="img-91c8bd3d-7" class="color_SoilMoisture">SoilMoisture</text>
      <text x="121.27" y="48.3" dy="0.35em" id="img-91c8bd3d-8" class="color_t2m">t2m</text>
    </g>
    <g stroke="#000000" stroke-opacity="0.000" id="img-91c8bd3d-9">
      <rect x="118.45" y="40.14" width="1.81" height="1.81" id="img-91c8bd3d-10" class="color_Emission" fill="#00BFFF"/>
      <rect x="118.45" y="43.76" width="1.81" height="1.81" id="img-91c8bd3d-11" class="color_SoilMoisture" fill="#D4CA3A"/>
      <rect x="118.45" y="47.39" width="1.81" height="1.81" id="img-91c8bd3d-12" class="color_t2m" fill="#FF6DAE"/>
    </g>
    <g fill="#362A35" font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" stroke="#000000" stroke-opacity="0.000" id="img-91c8bd3d-13">
      <text x="118.45" y="37.22" id="img-91c8bd3d-14">Color</text>
    </g>
  </g>
<g clip-path="url(#img-91c8bd3d-15)">
  <g id="img-91c8bd3d-16">
    <g pointer-events="visible" opacity="1" fill="#000000" fill-opacity="0.000" stroke="#000000" stroke-opacity="0.000" class="guide background" id="img-91c8bd3d-17">
      <rect x="17.27" y="5" width="100.19" height="75.72" id="img-91c8bd3d-18"/>
    </g>
    <g class="guide ygridlines xfixed" stroke-dasharray="0.5,0.5" stroke-width="0.2" stroke="#D0D0E0" id="img-91c8bd3d-19">
      <path fill="none" d="M17.27,164.77 L 117.45 164.77" id="img-91c8bd3d-20" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,150.43 L 117.45 150.43" id="img-91c8bd3d-21" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,136.09 L 117.45 136.09" id="img-91c8bd3d-22" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,121.74 L 117.45 121.74" id="img-91c8bd3d-23" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,107.4 L 117.45 107.4" id="img-91c8bd3d-24" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,93.06 L 117.45 93.06" id="img-91c8bd3d-25" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,78.72 L 117.45 78.72" id="img-91c8bd3d-26" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,64.37 L 117.45 64.37" id="img-91c8bd3d-27" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,50.03 L 117.45 50.03" id="img-91c8bd3d-28" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,35.69 L 117.45 35.69" id="img-91c8bd3d-29" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,21.34 L 117.45 21.34" id="img-91c8bd3d-30" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,7 L 117.45 7" id="img-91c8bd3d-31" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.27,-7.34 L 117.45 -7.34" id="img-91c8bd3d-32" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-21.69 L 117.45 -21.69" id="img-91c8bd3d-33" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-36.03 L 117.45 -36.03" id="img-91c8bd3d-34" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-50.37 L 117.45 -50.37" id="img-91c8bd3d-35" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-64.71 L 117.45 -64.71" id="img-91c8bd3d-36" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-79.06 L 117.45 -79.06" id="img-91c8bd3d-37" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.27,150.43 L 117.45 150.43" id="img-91c8bd3d-38" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,147.56 L 117.45 147.56" id="img-91c8bd3d-39" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,144.69 L 117.45 144.69" id="img-91c8bd3d-40" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,141.82 L 117.45 141.82" id="img-91c8bd3d-41" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,138.96 L 117.45 138.96" id="img-91c8bd3d-42" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,136.09 L 117.45 136.09" id="img-91c8bd3d-43" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,133.22 L 117.45 133.22" id="img-91c8bd3d-44" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,130.35 L 117.45 130.35" id="img-91c8bd3d-45" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,127.48 L 117.45 127.48" id="img-91c8bd3d-46" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,124.61 L 117.45 124.61" id="img-91c8bd3d-47" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,121.74 L 117.45 121.74" id="img-91c8bd3d-48" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,118.88 L 117.45 118.88" id="img-91c8bd3d-49" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,116.01 L 117.45 116.01" id="img-91c8bd3d-50" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,113.14 L 117.45 113.14" id="img-91c8bd3d-51" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,110.27 L 117.45 110.27" id="img-91c8bd3d-52" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,107.4 L 117.45 107.4" id="img-91c8bd3d-53" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,104.53 L 117.45 104.53" id="img-91c8bd3d-54" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,101.66 L 117.45 101.66" id="img-91c8bd3d-55" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,98.8 L 117.45 98.8" id="img-91c8bd3d-56" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,95.93 L 117.45 95.93" id="img-91c8bd3d-57" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,93.06 L 117.45 93.06" id="img-91c8bd3d-58" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,90.19 L 117.45 90.19" id="img-91c8bd3d-59" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,87.32 L 117.45 87.32" id="img-91c8bd3d-60" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,84.45 L 117.45 84.45" id="img-91c8bd3d-61" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,81.58 L 117.45 81.58" id="img-91c8bd3d-62" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,78.72 L 117.45 78.72" id="img-91c8bd3d-63" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,75.85 L 117.45 75.85" id="img-91c8bd3d-64" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,72.98 L 117.45 72.98" id="img-91c8bd3d-65" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,70.11 L 117.45 70.11" id="img-91c8bd3d-66" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,67.24 L 117.45 67.24" id="img-91c8bd3d-67" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,64.37 L 117.45 64.37" id="img-91c8bd3d-68" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,61.5 L 117.45 61.5" id="img-91c8bd3d-69" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,58.63 L 117.45 58.63" id="img-91c8bd3d-70" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,55.77 L 117.45 55.77" id="img-91c8bd3d-71" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,52.9 L 117.45 52.9" id="img-91c8bd3d-72" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,50.03 L 117.45 50.03" id="img-91c8bd3d-73" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,47.16 L 117.45 47.16" id="img-91c8bd3d-74" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,44.29 L 117.45 44.29" id="img-91c8bd3d-75" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,41.42 L 117.45 41.42" id="img-91c8bd3d-76" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,38.55 L 117.45 38.55" id="img-91c8bd3d-77" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,35.69 L 117.45 35.69" id="img-91c8bd3d-78" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,32.82 L 117.45 32.82" id="img-91c8bd3d-79" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,29.95 L 117.45 29.95" id="img-91c8bd3d-80" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,27.08 L 117.45 27.08" id="img-91c8bd3d-81" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,24.21 L 117.45 24.21" id="img-91c8bd3d-82" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,21.34 L 117.45 21.34" id="img-91c8bd3d-83" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,18.47 L 117.45 18.47" id="img-91c8bd3d-84" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,15.61 L 117.45 15.61" id="img-91c8bd3d-85" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,12.74 L 117.45 12.74" id="img-91c8bd3d-86" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,9.87 L 117.45 9.87" id="img-91c8bd3d-87" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,7 L 117.45 7" id="img-91c8bd3d-88" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,4.13 L 117.45 4.13" id="img-91c8bd3d-89" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,1.26 L 117.45 1.26" id="img-91c8bd3d-90" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-1.61 L 117.45 -1.61" id="img-91c8bd3d-91" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-4.47 L 117.45 -4.47" id="img-91c8bd3d-92" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-7.34 L 117.45 -7.34" id="img-91c8bd3d-93" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-10.21 L 117.45 -10.21" id="img-91c8bd3d-94" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-13.08 L 117.45 -13.08" id="img-91c8bd3d-95" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-15.95 L 117.45 -15.95" id="img-91c8bd3d-96" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-18.82 L 117.45 -18.82" id="img-91c8bd3d-97" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-21.69 L 117.45 -21.69" id="img-91c8bd3d-98" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-24.55 L 117.45 -24.55" id="img-91c8bd3d-99" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-27.42 L 117.45 -27.42" id="img-91c8bd3d-100" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-30.29 L 117.45 -30.29" id="img-91c8bd3d-101" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-33.16 L 117.45 -33.16" id="img-91c8bd3d-102" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-36.03 L 117.45 -36.03" id="img-91c8bd3d-103" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-38.9 L 117.45 -38.9" id="img-91c8bd3d-104" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-41.77 L 117.45 -41.77" id="img-91c8bd3d-105" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-44.63 L 117.45 -44.63" id="img-91c8bd3d-106" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-47.5 L 117.45 -47.5" id="img-91c8bd3d-107" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-50.37 L 117.45 -50.37" id="img-91c8bd3d-108" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-53.24 L 117.45 -53.24" id="img-91c8bd3d-109" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-56.11 L 117.45 -56.11" id="img-91c8bd3d-110" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-58.98 L 117.45 -58.98" id="img-91c8bd3d-111" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-61.85 L 117.45 -61.85" id="img-91c8bd3d-112" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-64.71 L 117.45 -64.71" id="img-91c8bd3d-113" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.27,164.77 L 117.45 164.77" id="img-91c8bd3d-114" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.27,107.4 L 117.45 107.4" id="img-91c8bd3d-115" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.27,50.03 L 117.45 50.03" id="img-91c8bd3d-116" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.27,-7.34 L 117.45 -7.34" id="img-91c8bd3d-117" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.27,-64.71 L 117.45 -64.71" id="img-91c8bd3d-118" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.27,153.3 L 117.45 153.3" id="img-91c8bd3d-119" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,147.56 L 117.45 147.56" id="img-91c8bd3d-120" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,141.82 L 117.45 141.82" id="img-91c8bd3d-121" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,136.09 L 117.45 136.09" id="img-91c8bd3d-122" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,130.35 L 117.45 130.35" id="img-91c8bd3d-123" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,124.61 L 117.45 124.61" id="img-91c8bd3d-124" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,118.88 L 117.45 118.88" id="img-91c8bd3d-125" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,113.14 L 117.45 113.14" id="img-91c8bd3d-126" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,107.4 L 117.45 107.4" id="img-91c8bd3d-127" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,101.66 L 117.45 101.66" id="img-91c8bd3d-128" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,95.93 L 117.45 95.93" id="img-91c8bd3d-129" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,90.19 L 117.45 90.19" id="img-91c8bd3d-130" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,84.45 L 117.45 84.45" id="img-91c8bd3d-131" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,78.72 L 117.45 78.72" id="img-91c8bd3d-132" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,72.98 L 117.45 72.98" id="img-91c8bd3d-133" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,67.24 L 117.45 67.24" id="img-91c8bd3d-134" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,61.5 L 117.45 61.5" id="img-91c8bd3d-135" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,55.77 L 117.45 55.77" id="img-91c8bd3d-136" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,50.03 L 117.45 50.03" id="img-91c8bd3d-137" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,44.29 L 117.45 44.29" id="img-91c8bd3d-138" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,38.55 L 117.45 38.55" id="img-91c8bd3d-139" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,32.82 L 117.45 32.82" id="img-91c8bd3d-140" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,27.08 L 117.45 27.08" id="img-91c8bd3d-141" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,21.34 L 117.45 21.34" id="img-91c8bd3d-142" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,15.61 L 117.45 15.61" id="img-91c8bd3d-143" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,9.87 L 117.45 9.87" id="img-91c8bd3d-144" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,4.13 L 117.45 4.13" id="img-91c8bd3d-145" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-1.61 L 117.45 -1.61" id="img-91c8bd3d-146" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-7.34 L 117.45 -7.34" id="img-91c8bd3d-147" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-13.08 L 117.45 -13.08" id="img-91c8bd3d-148" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-18.82 L 117.45 -18.82" id="img-91c8bd3d-149" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-24.55 L 117.45 -24.55" id="img-91c8bd3d-150" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-30.29 L 117.45 -30.29" id="img-91c8bd3d-151" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-36.03 L 117.45 -36.03" id="img-91c8bd3d-152" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-41.77 L 117.45 -41.77" id="img-91c8bd3d-153" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-47.5 L 117.45 -47.5" id="img-91c8bd3d-154" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-53.24 L 117.45 -53.24" id="img-91c8bd3d-155" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-58.98 L 117.45 -58.98" id="img-91c8bd3d-156" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.27,-64.71 L 117.45 -64.71" id="img-91c8bd3d-157" gadfly:scale="5.0" visibility="hidden"/>
    </g>
    <g class="guide xgridlines yfixed" stroke-dasharray="0.5,0.5" stroke-width="0.2" stroke="#D0D0E0" id="img-91c8bd3d-158">
      <path fill="none" d="M-108.98,5 L -108.98 80.72" id="img-91c8bd3d-159" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-76.91,5 L -76.91 80.72" id="img-91c8bd3d-160" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-44.85,5 L -44.85 80.72" id="img-91c8bd3d-161" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-12.79,5 L -12.79 80.72" id="img-91c8bd3d-162" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M19.27,5 L 19.27 80.72" id="img-91c8bd3d-163" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M51.34,5 L 51.34 80.72" id="img-91c8bd3d-164" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M83.4,5 L 83.4 80.72" id="img-91c8bd3d-165" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M115.45,5 L 115.45 80.72" id="img-91c8bd3d-166" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M147.51,5 L 147.51 80.72" id="img-91c8bd3d-167" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M179.59,5 L 179.59 80.72" id="img-91c8bd3d-168" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M211.64,5 L 211.64 80.72" id="img-91c8bd3d-169" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M243.7,5 L 243.7 80.72" id="img-91c8bd3d-170" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-108.98,5 L -108.98 80.72" id="img-91c8bd3d-171" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M-44.85,5 L -44.85 80.72" id="img-91c8bd3d-172" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M19.27,5 L 19.27 80.72" id="img-91c8bd3d-173" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M83.4,5 L 83.4 80.72" id="img-91c8bd3d-174" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M147.51,5 L 147.51 80.72" id="img-91c8bd3d-175" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M211.64,5 L 211.64 80.72" id="img-91c8bd3d-176" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M-108.98,5 L -108.98 80.72" id="img-91c8bd3d-177" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M-44.85,5 L -44.85 80.72" id="img-91c8bd3d-178" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M19.27,5 L 19.27 80.72" id="img-91c8bd3d-179" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M83.4,5 L 83.4 80.72" id="img-91c8bd3d-180" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M147.51,5 L 147.51 80.72" id="img-91c8bd3d-181" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M211.64,5 L 211.64 80.72" id="img-91c8bd3d-182" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M-108.98,5 L -108.98 80.72" id="img-91c8bd3d-183" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M-44.85,5 L -44.85 80.72" id="img-91c8bd3d-184" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M19.27,5 L 19.27 80.72" id="img-91c8bd3d-185" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M83.4,5 L 83.4 80.72" id="img-91c8bd3d-186" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M147.51,5 L 147.51 80.72" id="img-91c8bd3d-187" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M211.64,5 L 211.64 80.72" id="img-91c8bd3d-188" gadfly:scale="9.131666666666666" visibility="hidden"/>
    </g>
    <g class="plotpanel" id="img-91c8bd3d-189">
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry color_t2m" stroke-dasharray="none" stroke="#FF6DAE" id="img-91c8bd3d-190">
        <path fill="none" d="M19.27,50.03 L 19.41 50.03 19.55 50.03 19.69 50.03 19.83 50.03 19.97 50.03 20.11 50.03 20.25 50.03 20.39 50.03 20.53 50.03 20.67 50.03 20.81 50.03 20.95 50.03 21.09 50.03 21.23 50.03 21.37 50.03 21.51 50.03 21.65 50.03 21.79 50.03 21.93 50.03 22.07 50.03 22.22 50.03 22.36 50.03 22.5 50.03 22.64 50.03 22.78 50.03 22.92 50.03 23.06 50.03 23.2 50.03 23.34 50.03 23.48 50.03 23.62 50.03 23.76 50.03 23.9 50.03 24.04 50.03 24.18 50.03 24.32 50.03 24.46 50.03 24.6 50.03 24.74 50.03 24.88 50.03 25.02 50.03 25.16 50.03 25.31 50.03 25.45 50.03 25.59 50.03 25.69 48.03 25.83 49.77 25.97 48.28 26.11 49.71 26.25 47.94 26.39 47.77 26.53 47.72 26.67 48.63 26.81 44.13 26.96 47.45 27.1 51.63 27.24 48.41 27.38 48.57 27.52 47.93 27.66 49.17 27.8 53.21 27.94 51.02 28.08 50.56 28.22 50.13 28.36 48.55 28.5 47.3 28.64 48.93 28.78 48.22 28.92 48.66 29.06 47.83 29.2 48.83 29.34 48.57 29.48 49.22 29.62 47.25 29.76 50.1 29.9 50.81 30.05 51.7 30.19 50.16 30.33 52.42 30.47 50.85 30.61 52.35 30.75 50.13 30.89 54.16 31.03 54.99 31.17 53.38 31.31 47.2 31.45 49.49 31.59 51.46 31.73 55.53 31.87 53.54 32.01 47.12 32.1 47.01 32.24 48.51 32.38 52.17 32.52 50.2 32.66 52.2 32.8 55.61 32.94 53.74 33.08 49.88 33.22 48.63 33.36 43.96 33.5 45.99 33.64 49.79 33.78 51.39 33.93 49.5 34.07 48.24 34.21 47.69 34.35 48.89 34.49 48.71 34.63 49.25 34.77 49.78 34.91 51.62 35.05 49.42 35.19 50.44 35.33 50.64 35.47 50.92 35.61 49.31 35.75 50.01 35.89 50.44 36.03 49.43 36.17 50.87 36.31 49.8 36.45 50.24 36.59 52.4 36.73 49.01 36.87 47.38 37.01 47.16 37.16 45.4 37.3 45.82 37.44 43.35 37.58 43.54 37.72 46.33 37.86 45.84 38 46.76 38.14 49.26 38.28 49.6 38.42 49.14 38.51 51.2 38.65 45.88 38.79 45.81 38.93 47.52 39.07 44.47 39.21 47.6 39.35 46.44 39.49 44.88 39.63 48.3 39.77 54.37 39.91 47.05 40.05 45.81 40.19 48.85 40.33 48.21 40.47 47.73 40.61 46.83 40.75 49.55 40.89 47.87 41.04 48.98 41.18 49.16 41.32 48.79 41.46 49.82 41.6 49.99 41.74 49.49 41.88 49.82 42.02 48.83 42.16 49.3 42.3 47.7 42.44 50.41 42.58 49.81 42.72 49.62 42.86 48.31 43 49.59 43.14 48.54 43.28 47.25 43.42 50.11 43.56 52.69 43.7 47.41 43.84 43.2 43.98 47.36 44.13 46.51 44.27 52.84 44.41 52.27 44.55 48.3 44.69 51.22 44.83 46.19 44.92 48.91 45.06 54.63 45.2 50.3 45.34 47.79 45.48 47.92 45.62 49.11 45.76 49.82 45.9 47.76 46.04 49.1 46.18 48.86 46.32 46.49 46.46 55.7 46.6 48.26 46.74 48.53 46.88 48.59 47.02 48.37 47.16 48.64 47.3 48.3 47.44 48.78 47.58 49.53 47.72 50.51 47.86 49.57 48 49.44 48.15 48.62 48.29 48.05 48.43 46.57 48.57 47.98 48.71 47.3 48.85 47.07 48.99 45.96 49.13 47.54 49.27 47.27 49.41 46.6 49.55 48.22 49.69 47.68 49.83 47.56 49.97 46.67 50.11 47.49 50.25 51.76 50.39 46.69 50.53 46.44 50.67 45.85 50.81 51.02 50.95 46.56 51.09 47.14 51.24 47.33 51.34 44.74 51.48 45.33 51.62 45.75 51.76 45.74 51.9 48.13 52.04 48.49 52.18 46.71 52.32 47.53 52.46 43.93 52.6 42.43 52.74 46.07 52.89 53.66 53.03 49.97 53.17 46.98 53.31 46.12 53.45 46.41 53.59 45.77 53.73 48.03 53.87 48.24 54.01 47.59 54.15 46.03 54.29 47.41 54.43 46.98 54.57 46.96 54.71 46.95 54.85 47.86 54.99 46.5 55.13 47.07 55.27 46.68 55.41 46.97 55.55 48.65 55.69 47.89 55.83 48.38 55.98 48.1 56.12 50.76 56.26 50.17 56.4 49.06 56.54 52.51 56.68 48.41 56.82 53.79 56.96 55.26 57.1 52.1 57.24 46.76 57.38 45.82 57.52 45.94 57.66 47.36 57.75 47.17 57.89 49.89 58.03 54.18 58.17 60.77 58.31 54.2 58.45 48.18 58.59 48.91 58.73 52.7 58.87 48.53 59.01 46.26 59.15 48.07 59.29 46.28 59.43 48.83 59.57 53.58 59.71 54.28 59.86 50.99 60 50.78 60.14 50.71 60.28 51.38 60.42 49.17 60.56 50.15 60.7 50.04 60.84 50.1 60.98 50.53 61.12 51.23 61.26 51.59 61.4 51.63 61.54 53.08 61.68 52.6 61.82 50.88 61.96 50.5 62.1 51.21 62.24 51.02 62.38 51.76 62.52 48.64 62.66 48.3 62.8 48.71 62.94 50.4 63.09 52.34 63.23 54.43 63.37 55 63.51 49.34 63.65 50.82 63.79 52.01 63.93 54.68 64.07 52.98 64.16 55.82 64.3 49.26 64.44 53.29 64.58 46.41 64.72 47.13 64.86 49.25 65 52.3 65.14 54.2 65.28 64.55 65.42 54.96 65.56 54.65 65.7 49.04 65.84 50.41 65.98 51.4 66.12 51.28 66.26 50.46 66.4 51.2 66.54 51.02 66.68 52.21 66.82 51.43 66.97 50.94 67.11 49.59 67.25 49.65 67.39 48.49 67.53 50.99 67.67 49.58 67.81 50.04 67.95 49.22 68.09 49.04 68.23 48.83 68.37 48.47 68.51 48.43 68.65 48.7 68.79 49.45 68.93 50.56 69.07 52.5 69.21 54.51 69.35 48.71 69.49 49.45 69.63 51.02 69.77 50.28 69.91 46.45 70.06 49.87 70.2 51.24 70.34 51.44 70.48 50.28 70.56 58.49 70.7 59.81 70.85 48.63 70.99 49.84 71.13 50.95 71.27 52.89 71.41 53.85 71.55 50.75 71.69 49.36 71.83 51.47 71.97 60.35 72.11 47.98 72.25 51.62 72.39 50.74 72.53 53.23 72.67 54.75 72.81 52.21 72.95 52.23 73.09 51.17 73.23 52.4 73.37 52.59 73.51 51.34 73.65 51.46 73.79 52.62 73.93 50.79 74.08 52.02 74.22 50.76 74.36 49.77 74.5 50.95 74.64 51.73 74.78 52.6 74.92 52.92 75.06 53.04 75.2 51.24 75.34 52.95 75.48 51.04 75.62 52.97 75.76 52.3 75.9 52.42 76.04 50.59 76.18 50.36 76.32 54.26 76.46 49.11 76.6 46.9 76.74 46.64 76.88 60.42 76.99 53.87 77.13 48.2 77.27 48.7 77.41 53.35 77.55 53.65 77.69 51.02 77.83 50.46 77.97 48.75 78.11 47.96 78.25 60.36 78.39 51.43 78.53 50.39 78.68 51.97 78.82 51.85 78.96 48.51 79.1 49.11 79.24 50.76 79.38 49.73 79.52 48.71 79.66 50.53 79.8 50.69 79.94 51.31 80.08 50.83 80.22 49.82 80.36 49.47 80.5 51.7 80.64 52.52 80.78 53.42 80.92 53.4 81.06 53.03 81.2 52.26 81.34 51.87 81.48 50.91 81.62 51.75 81.76 49.48 81.91 47.91 82.05 49.42 82.19 52.06 82.33 51.65 82.47 51.12 82.61 55.85 82.75 54.11 82.89 48.99 83.03 52.29 83.17 45.6 83.31 46.74 83.4 45.06 83.54 49.01 83.68 53.17 83.82 48.95 83.96 53.69 84.1 50.38 84.24 50.33 84.38 55.22 84.52 55.8 84.66 50.16 84.8 48.55 84.94 53.22 85.08 50.43 85.22 51.58 85.36 53.15 85.5 52.47 85.64 51.45 85.79 53.15 85.93 51.44 86.07 52.14 86.21 51.67 86.35 52.86 86.49 53.18 86.63 54.47 86.77 54.24 86.91 54.01 87.05 52.98 87.19 53.08 87.33 53.45 87.47 52.11 87.61 50.04 87.75 50.46 87.89 49.48 88.03 49.8 88.17 54.74 88.31 53.2 88.45 50.73 88.59 49.43 88.73 52.72 88.88 48.37 89.02 47.05 89.16 50.01 89.3 53.22 89.44 52.38 89.58 54.5 89.72 52.73" id="img-91c8bd3d-191"/>
      </g>
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry color_SoilMoisture" stroke-dasharray="none" stroke="#D4CA3A" id="img-91c8bd3d-192">
        <path fill="none" d="M19.27,50.03 L 19.41 50.03 19.55 50.03 19.69 50.03 19.83 50.03 19.97 50.03 20.11 50.03 20.25 50.03 20.39 50.03 20.53 50.03 20.67 50.03 20.81 42.37 20.95 42.14 21.09 50.12 21.23 57.38 21.37 47.59 21.51 51.03 21.65 52.68 21.79 48.83 21.93 52.74 22.07 49.67 22.22 50.82 22.36 51.42 22.5 50.58 22.64 52.41 22.78 50.53 22.92 49.43 23.06 49.14 23.2 51.37 23.34 46.68 23.48 49.12 23.62 49.62 23.76 39.44 23.9 49.39 24.04 53.4 24.18 52.57 24.32 51.28 24.46 47.83 24.6 47.23 24.74 50.03 24.88 50.03 25.02 50.03 25.16 50.03 25.31 50.03 25.45 50.03 25.59 50.03 25.69 50.03 25.83 50.03 25.97 50.03 26.11 50.03 26.25 50.03 26.39 50.03 26.53 50.03 26.67 50.03 26.81 50.03 26.96 50.03 27.1 50.03 27.24 52.47 27.38 60.04 27.52 48.4 27.66 50.03 27.8 48.56 27.94 51.5 28.08 50.03 28.22 48.47 28.36 50.03 28.5 50.03 28.64 50.03 28.78 50.03 28.92 47.07 29.06 48.29 29.2 47.72 29.34 45.84 29.48 44 29.62 46.6 29.76 47.31 29.9 48.72 30.05 48.03 30.19 47.25 30.33 50.44 30.47 47.54 30.61 57.77 30.75 50.03 30.89 49.9 31.03 50.68 31.17 61.57 31.31 50.03 31.45 50.03 31.59 50.03 31.73 50.03 31.87 50.03 32.01 50.03 32.1 50.03 32.24 50.03 32.38 50.03 32.52 50.03 32.66 50.03 32.8 50.03 32.94 50.03 33.08 50.03 33.22 50.03 33.36 50.03 33.5 50.03 33.64 54 33.78 51.94 33.93 47.99 34.07 47.9 34.21 50.67 34.35 52.77 34.49 47.31 34.63 43.63 34.77 48.02 34.91 48.54 35.05 52.53 35.19 53.22 35.33 48.91 35.47 51.26 35.61 52.04 35.75 52.58 35.89 52.27 36.03 50.34 36.17 53.55 36.31 53.37 36.45 53.1 36.59 54.37 36.73 50.56 36.87 52.53 37.01 46.83 37.16 47.42 37.3 48.6 37.44 50.07 37.58 50.26 37.72 50.99 37.86 51.94 38 57 38.14 57.23 38.28 56.46 38.42 54.04 38.51 50.03 38.65 51.32 38.79 52.39 38.93 47.05 39.07 54.74 39.21 51.92 39.35 52.14 39.49 50.03 39.63 53.14 39.77 50.34 39.91 53.3 40.05 51.91 40.19 52.02 40.33 50.94 40.47 49.76 40.61 50.94 40.75 50.3 40.89 51.01 41.04 52.71 41.18 53.2 41.32 53.95 41.46 50.56 41.6 50.33 41.74 48.44 41.88 49.04 42.02 49.98 42.16 52.36 42.3 53.47 42.44 53.02 42.58 50.42 42.72 49.72 42.86 51.01 43 53.32 43.14 51.71 43.28 48.13 43.42 47.42 43.56 49.88 43.7 50.65 43.84 47.65 43.98 49.9 44.13 49.77 44.27 50.22 44.41 50 44.55 51.15 44.69 48.96 44.83 49.62 44.92 48.98 45.06 52.38 45.2 48.12 45.34 45.71 45.48 45.69 45.62 47.91 45.76 49.86 45.9 50.03 46.04 53.23 46.18 47.2 46.32 48.72 46.46 50.29 46.6 49.75 46.74 50.04 46.88 49.08 47.02 49.9 47.16 49.11 47.3 50.31 47.44 50.21 47.58 51.07 47.72 50.18 47.86 49.74 48 50.69 48.15 51.5 48.29 46.56 48.43 39.52 48.57 46.33 48.71 51.28 48.85 51.07 48.99 50.85 49.13 51.51 49.27 52.31 49.41 52.97 49.55 50.17 49.69 50.56 49.83 48.7 49.97 49.27 50.11 50.17 50.25 50.25 50.39 47.69 50.53 52.19 50.67 52.67 50.81 48.61 50.95 48.03 51.09 50.5 51.24 46.03 51.34 47.96 51.48 47.84 51.62 52.77 51.76 51.29 51.9 55.48 52.04 53.69 52.18 52.86 52.32 50.03 52.46 57.4 52.6 54.03 52.74 46.87 52.89 51.02 53.03 49.18 53.17 51.54 53.31 49.43 53.45 49.66 53.59 47.07 53.73 47.11 53.87 51.27 54.01 48.47 54.15 48.59 54.29 48.63 54.43 44.76 54.57 44.41 54.71 52.34 54.85 51.29 54.99 48.63 55.13 49.21 55.27 51.25 55.41 52.45 55.55 54.04 55.69 52.5 55.83 48.81 55.98 50.12 56.12 48.31 56.26 49.77 56.4 51.59 56.54 50.16 56.68 51.14 56.82 46.96 56.96 52.62 57.1 52.81 57.24 48.14 57.38 45.94 57.52 50.92 57.66 53.61 57.75 57.16 57.89 54.07 58.03 54.63 58.17 54 58.31 57.49 58.45 57.97 58.59 52.99 58.73 50.03 58.87 50.03 59.01 55.28 59.15 55.24 59.29 48.72 59.43 49.6 59.57 50.11 59.71 49.03 59.86 52.34 60 50.96 60.14 49.38 60.28 48.38 60.42 47.63 60.56 49.66 60.7 47.52 60.84 47.02 60.98 48.43 61.12 49.24 61.26 53.38 61.4 52.72 61.54 49.59 61.68 48.39 61.82 49.15 61.96 48.15 62.1 49.43 62.24 51.45 62.38 51.12 62.52 49.27 62.66 49.22 62.8 49.63 62.94 50.96 63.09 50.82 63.23 47.81 63.37 50.82 63.51 48.71 63.65 50.57 63.79 47.77 63.93 45.84 64.07 46.85 64.16 46.02 64.3 44.54 64.44 48.1 64.58 48.1 64.72 48.31 64.86 50.03 65 50.03 65.14 48.57 65.28 44.12 65.42 44.93 65.56 48.08 65.7 55.55 65.84 48.84 65.98 50.4 66.12 51.37 66.26 52.78 66.4 48.35 66.54 52.73 66.68 53.02 66.82 50.16 66.97 51.24 67.11 48.6 67.25 50.42 67.39 50.52 67.53 49.48 67.67 52.51 67.81 48.98 67.95 49.05 68.09 50.35 68.23 50.11 68.37 49.53 68.51 49.9 68.65 51.14 68.79 49.21 68.93 51.62 69.07 48.37 69.21 50.59 69.35 54.33 69.49 50.48 69.63 47.36 69.77 50.03 69.91 47.59 70.06 49.18 70.2 50.08 70.34 50.03 70.48 50.03 70.56 50.03 70.7 50.03 70.85 44.48 70.99 57.42 71.13 48.2 71.27 43.24 71.41 43.96 71.55 47.64 71.69 44.54 71.83 48.42 71.97 51.97 72.11 48.38 72.25 49.29 72.39 47.44 72.53 47.85 72.67 48.26 72.81 49.13 72.95 50.66 73.09 53.34 73.23 52.21 73.37 51.74 73.51 51.61 73.65 51.24 73.79 51.94 73.93 50.17 74.08 52.32 74.22 52.78 74.36 50.79 74.5 48.07 74.64 50.19 74.78 50.31 74.92 48.89 75.06 51.64 75.2 47.74 75.34 50.47 75.48 50.59 75.62 50.86 75.76 51.02 75.9 52.59 76.04 53.89 76.18 48.47 76.32 49.47 76.46 47.13 76.6 45.27 76.74 47.48 76.88 50.03 76.99 50.03 77.13 50.03 77.27 49.71 77.41 46.62 77.55 40.29 77.69 45.44 77.83 50.03 77.97 50.03 78.11 44.83 78.25 47.37 78.39 48.7 78.53 49.89 78.68 51.16 78.82 52.28 78.96 52.1 79.1 52.72 79.24 53.14 79.38 52.42 79.52 51.27 79.66 49.06 79.8 49.89 79.94 49.98 80.08 48.82 80.22 53.48 80.36 49.41 80.5 48.56 80.64 52.16 80.78 52.47 80.92 49.04 81.06 50.66 81.2 48.62 81.34 48.65 81.48 52.31 81.62 51.09 81.76 50.19 81.91 48.99 82.05 48.7 82.19 47.86 82.33 52.28 82.47 47.83 82.61 49.8 82.75 52.02 82.89 49.6 83.03 54.76 83.17 50.03 83.31 50.03 83.4 50.03 83.54 50.03 83.68 50.03 83.82 50.03 83.96 50.03 84.1 50.03 84.24 48.37 84.38 53.87 84.52 52.94 84.66 52.67 84.8 47.35 84.94 45.7 85.08 46.37 85.22 51.07 85.36 46.4 85.5 46.89 85.64 46.97 85.79 46.68 85.93 49.19 86.07 47.73 86.21 46.83 86.35 50.29 86.49 52.36 86.63 55.04 86.77 52.12 86.91 52.48 87.05 48.5 87.19 49.07 87.33 50.83 87.47 48.95 87.61 47.24 87.75 46.87 87.89 47.62 88.03 48.76 88.17 48.29 88.31 50.08 88.45 51.08 88.59 48.86 88.73 47.14 88.88 47.02 89.02 45.57 89.16 44.82 89.3 50.03 89.44 50.03 89.58 50.03 89.72 50.03" id="img-91c8bd3d-193"/>
      </g>
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry color_Emission" stroke-dasharray="none" stroke="#00BFFF" id="img-91c8bd3d-194">
        <path fill="none" d="M19.27,50.03 L 19.41 50.03 19.55 50.03 19.69 50.03 19.83 50.03 19.97 50.03 20.11 50.03 20.25 50.03 20.39 50.03 20.53 50.03 20.67 50.03 20.81 50.03 20.95 50.03 21.09 50.03 21.23 50.03 21.37 50.03 21.51 50.03 21.65 50.03 21.79 50.03 21.93 50.03 22.07 50.03 22.22 50.03 22.36 50.03 22.5 50.03 22.64 50.03 22.78 50.03 22.92 50.03 23.06 50.03 23.2 50.03 23.34 50.03 23.48 50.03 23.62 50.03 23.76 50.03 23.9 50.03 24.04 50.03 24.18 50.03 24.32 50.03 24.46 50.03 24.6 50.03 24.74 50.03 24.88 50.03 25.02 50.03 25.16 50.03 25.31 50.03 25.45 50.03 25.59 50.03 25.69 50.03 25.83 50.03 25.97 50.03 26.11 50.03 26.25 50.03 26.39 50.03 26.53 50.03 26.67 50.03 26.81 50.03 26.96 50.03 27.1 50.03 27.24 51.4 27.38 53.69 27.52 53.69 27.66 53.69 27.8 50.03 27.94 50.03 28.08 50.03 28.22 50.03 28.36 50.03 28.5 50.03 28.64 50.03 28.78 50.03 28.92 50.03 29.06 50.03 29.2 50.03 29.34 50.03 29.48 50.03 29.62 50.03 29.76 50.03 29.9 50.03 30.05 50.03 30.19 50.03 30.33 50.03 30.47 50.03 30.61 50.03 30.75 50.03 30.89 50.03 31.03 50.03 31.17 50.03 31.31 50.03 31.45 50.03 31.59 50.03 31.73 50.03 31.87 50.03 32.01 50.03 32.1 50.03 32.24 50.03 32.38 50.03 32.52 50.03 32.66 50.03 32.8 50.03 32.94 50.03 33.08 50.03 33.22 50.03 33.36 50.03 33.5 50.03 33.64 51.4 33.78 53.69 33.93 53.69 34.07 53.69 34.21 50.03 34.35 50.03 34.49 50.03 34.63 50.03 34.77 50.03 34.91 50.03 35.05 50.03 35.19 50.03 35.33 50.03 35.47 50.03 35.61 50.03 35.75 50.03 35.89 50.03 36.03 50.03 36.17 50.03 36.31 50.03 36.45 50.03 36.59 50.03 36.73 50.03 36.87 50.03 37.01 50.03 37.16 50.03 37.3 50.03 37.44 50.03 37.58 50.03 37.72 50.03 37.86 50.03 38 50.03 38.14 50.03 38.28 50.03 38.42 50.03 38.51 50.03 38.65 50.03 38.79 50.03 38.93 50.03 39.07 50.03 39.21 50.03 39.35 50.03 39.49 50.03 39.63 50.03 39.77 50.03 39.91 50.03 40.05 51.4 40.19 53.69 40.33 53.69 40.47 53.69 40.61 50.03 40.75 50.03 40.89 50.03 41.04 50.03 41.18 50.03 41.32 50.03 41.46 50.03 41.6 50.03 41.74 50.03 41.88 50.03 42.02 50.03 42.16 50.03 42.3 50.03 42.44 50.03 42.58 50.03 42.72 50.03 42.86 50.03 43 50.03 43.14 50.03 43.28 50.03 43.42 50.03 43.56 50.03 43.7 50.03 43.84 50.03 43.98 50.03 44.13 50.03 44.27 50.03 44.41 50.03 44.55 50.03 44.69 50.03 44.83 50.03 44.92 50.03 45.06 50.03 45.2 50.03 45.34 50.03 45.48 50.03 45.62 50.03 45.76 50.03 45.9 50.03 46.04 50.03 46.18 50.03 46.32 50.03 46.46 51.4 46.6 53.69 46.74 53.69 46.88 53.69 47.02 50.03 47.16 50.03 47.3 50.03 47.44 50.03 47.58 50.03 47.72 50.03 47.86 50.03 48 50.03 48.15 50.03 48.29 50.03 48.43 50.03 48.57 50.03 48.71 50.03 48.85 50.03 48.99 50.03 49.13 50.03 49.27 50.03 49.41 50.03 49.55 50.03 49.69 50.03 49.83 50.03 49.97 50.03 50.11 50.03 50.25 50.03 50.39 50.03 50.53 50.03 50.67 50.03 50.81 50.03 50.95 50.03 51.09 50.03 51.24 50.03 51.34 50.03 51.48 50.03 51.62 50.03 51.76 50.03 51.9 50.03 52.04 50.03 52.18 50.03 52.32 50.03 52.46 50.03 52.6 50.03 52.74 50.03 52.89 51.4 53.03 53.69 53.17 53.69 53.31 53.69 53.45 50.03 53.59 50.03 53.73 50.03 53.87 50.03 54.01 50.03 54.15 50.03 54.29 50.03 54.43 50.03 54.57 50.03 54.71 50.03 54.85 50.03 54.99 50.03 55.13 50.03 55.27 50.03 55.41 50.03 55.55 50.03 55.69 50.03 55.83 50.03 55.98 50.03 56.12 50.03 56.26 50.03 56.4 50.03 56.54 50.03 56.68 50.03 56.82 50.03 56.96 50.03 57.1 50.03 57.24 50.03 57.38 50.03 57.52 50.03 57.66 50.03 57.75 50.03 57.89 50.03 58.03 50.03 58.17 50.03 58.31 50.03 58.45 50.03 58.59 50.03 58.73 50.03 58.87 50.03 59.01 50.03 59.15 50.03 59.29 37.68 59.43 17.09 59.57 17.09 59.71 17.09 59.86 50.03 60 50.03 60.14 50.03 60.28 50.03 60.42 50.03 60.56 50.03 60.7 50.03 60.84 50.03 60.98 50.03 61.12 50.03 61.26 50.03 61.4 50.03 61.54 50.03 61.68 50.03 61.82 50.03 61.96 50.03 62.1 50.03 62.24 50.03 62.38 50.03 62.52 50.03 62.66 50.03 62.8 50.03 62.94 50.03 63.09 50.03 63.23 50.03 63.37 50.03 63.51 50.03 63.65 50.03 63.79 50.03 63.93 50.03 64.07 50.03 64.16 50.03 64.3 50.03 64.44 50.03 64.58 50.03 64.72 50.03 64.86 50.03 65 50.03 65.14 50.03 65.28 50.03 65.42 50.03 65.56 50.03 65.7 51.4 65.84 53.69 65.98 53.69 66.12 53.69 66.26 50.03 66.4 50.03 66.54 50.03 66.68 50.03 66.82 50.03 66.97 50.03 67.11 50.03 67.25 50.03 67.39 50.03 67.53 50.03 67.67 50.03 67.81 50.03 67.95 50.03 68.09 50.03 68.23 50.03 68.37 50.03 68.51 50.03 68.65 50.03 68.79 50.03 68.93 50.03 69.07 50.03 69.21 50.03 69.35 50.03 69.49 50.03 69.63 50.03 69.77 50.03 69.91 50.03 70.06 50.03 70.2 50.03 70.34 50.03 70.48 50.03 70.56 50.03 70.7 50.03 70.85 50.03 70.99 50.03 71.13 50.03 71.27 50.03 71.41 50.03 71.55 50.03 71.69 50.03 71.83 50.03 71.97 50.03 72.11 51.4 72.25 53.69 72.39 53.69 72.53 53.69 72.67 50.03 72.81 50.03 72.95 50.03 73.09 50.03 73.23 50.03 73.37 50.03 73.51 50.03 73.65 50.03 73.79 50.03 73.93 50.03 74.08 50.03 74.22 50.03 74.36 50.03 74.5 50.03 74.64 50.03 74.78 50.03 74.92 50.03 75.06 50.03 75.2 50.03 75.34 50.03 75.48 50.03 75.62 50.03 75.76 50.03 75.9 50.03 76.04 50.03 76.18 50.03 76.32 50.03 76.46 50.03 76.6 50.03 76.74 50.03 76.88 50.03 76.99 50.03 77.13 50.03 77.27 50.03 77.41 50.03 77.55 50.03 77.69 50.03 77.83 50.03 77.97 50.03 78.11 50.03 78.25 50.03 78.39 50.03 78.53 51.4 78.68 53.69 78.82 53.69 78.96 53.69 79.1 50.03 79.24 50.03 79.38 50.03 79.52 50.03 79.66 50.03 79.8 50.03 79.94 50.03 80.08 50.03 80.22 50.03 80.36 50.03 80.5 50.03 80.64 50.03 80.78 50.03 80.92 50.03 81.06 50.03 81.2 50.03 81.34 50.03 81.48 50.03 81.62 50.03 81.76 50.03 81.91 50.03 82.05 50.03 82.19 50.03 82.33 50.03 82.47 50.03 82.61 50.03 82.75 50.03 82.89 50.03 83.03 50.03 83.17 50.03 83.31 50.03 83.4 50.03 83.54 50.03 83.68 50.03 83.82 50.03 83.96 50.03 84.1 50.03 84.24 50.03 84.38 50.03 84.52 50.03 84.66 50.03 84.8 50.03 84.94 51.4 85.08 53.69 85.22 53.69 85.36 53.69 85.5 50.03 85.64 50.03 85.79 50.03 85.93 50.03 86.07 50.03 86.21 50.03 86.35 50.03 86.49 50.03 86.63 50.03 86.77 50.03 86.91 50.03 87.05 50.03 87.19 50.03 87.33 50.03 87.47 50.03 87.61 50.03 87.75 50.03 87.89 50.03 88.03 50.03 88.17 50.03 88.31 50.03 88.45 50.03 88.59 50.03 88.73 50.03 88.88 50.03 89.02 50.03 89.16 50.03 89.3 50.03 89.44 50.03 89.58 50.03 89.72 50.03" id="img-91c8bd3d-195"/>
      </g>
    </g>
    <g opacity="0" class="guide zoomslider" stroke="#000000" stroke-opacity="0.000" id="img-91c8bd3d-196">
      <g fill="#EAEAEA" stroke-width="0.3" stroke-opacity="0" stroke="#6A6A6A" id="img-91c8bd3d-197">
        <rect x="110.45" y="8" width="4" height="4" id="img-91c8bd3d-198"/>
        <g class="button_logo" fill="#6A6A6A" id="img-91c8bd3d-199">
          <path d="M111.25,9.6 L 112.05 9.6 112.05 8.8 112.85 8.8 112.85 9.6 113.65 9.6 113.65 10.4 112.85 10.4 112.85 11.2 112.05 11.2 112.05 10.4 111.25 10.4 z" id="img-91c8bd3d-200"/>
        </g>
      </g>
      <g fill="#EAEAEA" id="img-91c8bd3d-201">
        <rect x="90.95" y="8" width="19" height="4" id="img-91c8bd3d-202"/>
      </g>
      <g class="zoomslider_thumb" fill="#6A6A6A" id="img-91c8bd3d-203">
        <rect x="99.45" y="8" width="2" height="4" id="img-91c8bd3d-204"/>
      </g>
      <g fill="#EAEAEA" stroke-width="0.3" stroke-opacity="0" stroke="#6A6A6A" id="img-91c8bd3d-205">
        <rect x="86.45" y="8" width="4" height="4" id="img-91c8bd3d-206"/>
        <g class="button_logo" fill="#6A6A6A" id="img-91c8bd3d-207">
          <path d="M87.25,9.6 L 89.65 9.6 89.65 10.4 87.25 10.4 z" id="img-91c8bd3d-208"/>
        </g>
      </g>
    </g>
  </g>
</g>
  <g class="guide ylabels" font-size="2.82" font-family="'PT Sans Caption','Helvetica Neue','Helvetica',sans-serif" fill="#6C606B" id="img-91c8bd3d-209">
    <text x="16.27" y="164.77" text-anchor="end" dy="0.35em" id="img-91c8bd3d-210" gadfly:scale="1.0" visibility="hidden">-40</text>
    <text x="16.27" y="150.43" text-anchor="end" dy="0.35em" id="img-91c8bd3d-211" gadfly:scale="1.0" visibility="hidden">-35</text>
    <text x="16.27" y="136.09" text-anchor="end" dy="0.35em" id="img-91c8bd3d-212" gadfly:scale="1.0" visibility="hidden">-30</text>
    <text x="16.27" y="121.74" text-anchor="end" dy="0.35em" id="img-91c8bd3d-213" gadfly:scale="1.0" visibility="hidden">-25</text>
    <text x="16.27" y="107.4" text-anchor="end" dy="0.35em" id="img-91c8bd3d-214" gadfly:scale="1.0" visibility="hidden">-20</text>
    <text x="16.27" y="93.06" text-anchor="end" dy="0.35em" id="img-91c8bd3d-215" gadfly:scale="1.0" visibility="hidden">-15</text>
    <text x="16.27" y="78.72" text-anchor="end" dy="0.35em" id="img-91c8bd3d-216" gadfly:scale="1.0" visibility="visible">-10</text>
    <text x="16.27" y="64.37" text-anchor="end" dy="0.35em" id="img-91c8bd3d-217" gadfly:scale="1.0" visibility="visible">-5</text>
    <text x="16.27" y="50.03" text-anchor="end" dy="0.35em" id="img-91c8bd3d-218" gadfly:scale="1.0" visibility="visible">0</text>
    <text x="16.27" y="35.69" text-anchor="end" dy="0.35em" id="img-91c8bd3d-219" gadfly:scale="1.0" visibility="visible">5</text>
    <text x="16.27" y="21.34" text-anchor="end" dy="0.35em" id="img-91c8bd3d-220" gadfly:scale="1.0" visibility="visible">10</text>
    <text x="16.27" y="7" text-anchor="end" dy="0.35em" id="img-91c8bd3d-221" gadfly:scale="1.0" visibility="visible">15</text>
    <text x="16.27" y="-7.34" text-anchor="end" dy="0.35em" id="img-91c8bd3d-222" gadfly:scale="1.0" visibility="hidden">20</text>
    <text x="16.27" y="-21.69" text-anchor="end" dy="0.35em" id="img-91c8bd3d-223" gadfly:scale="1.0" visibility="hidden">25</text>
    <text x="16.27" y="-36.03" text-anchor="end" dy="0.35em" id="img-91c8bd3d-224" gadfly:scale="1.0" visibility="hidden">30</text>
    <text x="16.27" y="-50.37" text-anchor="end" dy="0.35em" id="img-91c8bd3d-225" gadfly:scale="1.0" visibility="hidden">35</text>
    <text x="16.27" y="-64.71" text-anchor="end" dy="0.35em" id="img-91c8bd3d-226" gadfly:scale="1.0" visibility="hidden">40</text>
    <text x="16.27" y="-79.06" text-anchor="end" dy="0.35em" id="img-91c8bd3d-227" gadfly:scale="1.0" visibility="hidden">45</text>
    <text x="16.27" y="150.43" text-anchor="end" dy="0.35em" id="img-91c8bd3d-228" gadfly:scale="10.0" visibility="hidden">-35</text>
    <text x="16.27" y="147.56" text-anchor="end" dy="0.35em" id="img-91c8bd3d-229" gadfly:scale="10.0" visibility="hidden">-34</text>
    <text x="16.27" y="144.69" text-anchor="end" dy="0.35em" id="img-91c8bd3d-230" gadfly:scale="10.0" visibility="hidden">-33</text>
    <text x="16.27" y="141.82" text-anchor="end" dy="0.35em" id="img-91c8bd3d-231" gadfly:scale="10.0" visibility="hidden">-32</text>
    <text x="16.27" y="138.96" text-anchor="end" dy="0.35em" id="img-91c8bd3d-232" gadfly:scale="10.0" visibility="hidden">-31</text>
    <text x="16.27" y="136.09" text-anchor="end" dy="0.35em" id="img-91c8bd3d-233" gadfly:scale="10.0" visibility="hidden">-30</text>
    <text x="16.27" y="133.22" text-anchor="end" dy="0.35em" id="img-91c8bd3d-234" gadfly:scale="10.0" visibility="hidden">-29</text>
    <text x="16.27" y="130.35" text-anchor="end" dy="0.35em" id="img-91c8bd3d-235" gadfly:scale="10.0" visibility="hidden">-28</text>
    <text x="16.27" y="127.48" text-anchor="end" dy="0.35em" id="img-91c8bd3d-236" gadfly:scale="10.0" visibility="hidden">-27</text>
    <text x="16.27" y="124.61" text-anchor="end" dy="0.35em" id="img-91c8bd3d-237" gadfly:scale="10.0" visibility="hidden">-26</text>
    <text x="16.27" y="121.74" text-anchor="end" dy="0.35em" id="img-91c8bd3d-238" gadfly:scale="10.0" visibility="hidden">-25</text>
    <text x="16.27" y="118.88" text-anchor="end" dy="0.35em" id="img-91c8bd3d-239" gadfly:scale="10.0" visibility="hidden">-24</text>
    <text x="16.27" y="116.01" text-anchor="end" dy="0.35em" id="img-91c8bd3d-240" gadfly:scale="10.0" visibility="hidden">-23</text>
    <text x="16.27" y="113.14" text-anchor="end" dy="0.35em" id="img-91c8bd3d-241" gadfly:scale="10.0" visibility="hidden">-22</text>
    <text x="16.27" y="110.27" text-anchor="end" dy="0.35em" id="img-91c8bd3d-242" gadfly:scale="10.0" visibility="hidden">-21</text>
    <text x="16.27" y="107.4" text-anchor="end" dy="0.35em" id="img-91c8bd3d-243" gadfly:scale="10.0" visibility="hidden">-20</text>
    <text x="16.27" y="104.53" text-anchor="end" dy="0.35em" id="img-91c8bd3d-244" gadfly:scale="10.0" visibility="hidden">-19</text>
    <text x="16.27" y="101.66" text-anchor="end" dy="0.35em" id="img-91c8bd3d-245" gadfly:scale="10.0" visibility="hidden">-18</text>
    <text x="16.27" y="98.8" text-anchor="end" dy="0.35em" id="img-91c8bd3d-246" gadfly:scale="10.0" visibility="hidden">-17</text>
    <text x="16.27" y="95.93" text-anchor="end" dy="0.35em" id="img-91c8bd3d-247" gadfly:scale="10.0" visibility="hidden">-16</text>
    <text x="16.27" y="93.06" text-anchor="end" dy="0.35em" id="img-91c8bd3d-248" gadfly:scale="10.0" visibility="hidden">-15</text>
    <text x="16.27" y="90.19" text-anchor="end" dy="0.35em" id="img-91c8bd3d-249" gadfly:scale="10.0" visibility="hidden">-14</text>
    <text x="16.27" y="87.32" text-anchor="end" dy="0.35em" id="img-91c8bd3d-250" gadfly:scale="10.0" visibility="hidden">-13</text>
    <text x="16.27" y="84.45" text-anchor="end" dy="0.35em" id="img-91c8bd3d-251" gadfly:scale="10.0" visibility="hidden">-12</text>
    <text x="16.27" y="81.58" text-anchor="end" dy="0.35em" id="img-91c8bd3d-252" gadfly:scale="10.0" visibility="hidden">-11</text>
    <text x="16.27" y="78.72" text-anchor="end" dy="0.35em" id="img-91c8bd3d-253" gadfly:scale="10.0" visibility="hidden">-10</text>
    <text x="16.27" y="75.85" text-anchor="end" dy="0.35em" id="img-91c8bd3d-254" gadfly:scale="10.0" visibility="hidden">-9</text>
    <text x="16.27" y="72.98" text-anchor="end" dy="0.35em" id="img-91c8bd3d-255" gadfly:scale="10.0" visibility="hidden">-8</text>
    <text x="16.27" y="70.11" text-anchor="end" dy="0.35em" id="img-91c8bd3d-256" gadfly:scale="10.0" visibility="hidden">-7</text>
    <text x="16.27" y="67.24" text-anchor="end" dy="0.35em" id="img-91c8bd3d-257" gadfly:scale="10.0" visibility="hidden">-6</text>
    <text x="16.27" y="64.37" text-anchor="end" dy="0.35em" id="img-91c8bd3d-258" gadfly:scale="10.0" visibility="hidden">-5</text>
    <text x="16.27" y="61.5" text-anchor="end" dy="0.35em" id="img-91c8bd3d-259" gadfly:scale="10.0" visibility="hidden">-4</text>
    <text x="16.27" y="58.63" text-anchor="end" dy="0.35em" id="img-91c8bd3d-260" gadfly:scale="10.0" visibility="hidden">-3</text>
    <text x="16.27" y="55.77" text-anchor="end" dy="0.35em" id="img-91c8bd3d-261" gadfly:scale="10.0" visibility="hidden">-2</text>
    <text x="16.27" y="52.9" text-anchor="end" dy="0.35em" id="img-91c8bd3d-262" gadfly:scale="10.0" visibility="hidden">-1</text>
    <text x="16.27" y="50.03" text-anchor="end" dy="0.35em" id="img-91c8bd3d-263" gadfly:scale="10.0" visibility="hidden">0</text>
    <text x="16.27" y="47.16" text-anchor="end" dy="0.35em" id="img-91c8bd3d-264" gadfly:scale="10.0" visibility="hidden">1</text>
    <text x="16.27" y="44.29" text-anchor="end" dy="0.35em" id="img-91c8bd3d-265" gadfly:scale="10.0" visibility="hidden">2</text>
    <text x="16.27" y="41.42" text-anchor="end" dy="0.35em" id="img-91c8bd3d-266" gadfly:scale="10.0" visibility="hidden">3</text>
    <text x="16.27" y="38.55" text-anchor="end" dy="0.35em" id="img-91c8bd3d-267" gadfly:scale="10.0" visibility="hidden">4</text>
    <text x="16.27" y="35.69" text-anchor="end" dy="0.35em" id="img-91c8bd3d-268" gadfly:scale="10.0" visibility="hidden">5</text>
    <text x="16.27" y="32.82" text-anchor="end" dy="0.35em" id="img-91c8bd3d-269" gadfly:scale="10.0" visibility="hidden">6</text>
    <text x="16.27" y="29.95" text-anchor="end" dy="0.35em" id="img-91c8bd3d-270" gadfly:scale="10.0" visibility="hidden">7</text>
    <text x="16.27" y="27.08" text-anchor="end" dy="0.35em" id="img-91c8bd3d-271" gadfly:scale="10.0" visibility="hidden">8</text>
    <text x="16.27" y="24.21" text-anchor="end" dy="0.35em" id="img-91c8bd3d-272" gadfly:scale="10.0" visibility="hidden">9</text>
    <text x="16.27" y="21.34" text-anchor="end" dy="0.35em" id="img-91c8bd3d-273" gadfly:scale="10.0" visibility="hidden">10</text>
    <text x="16.27" y="18.47" text-anchor="end" dy="0.35em" id="img-91c8bd3d-274" gadfly:scale="10.0" visibility="hidden">11</text>
    <text x="16.27" y="15.61" text-anchor="end" dy="0.35em" id="img-91c8bd3d-275" gadfly:scale="10.0" visibility="hidden">12</text>
    <text x="16.27" y="12.74" text-anchor="end" dy="0.35em" id="img-91c8bd3d-276" gadfly:scale="10.0" visibility="hidden">13</text>
    <text x="16.27" y="9.87" text-anchor="end" dy="0.35em" id="img-91c8bd3d-277" gadfly:scale="10.0" visibility="hidden">14</text>
    <text x="16.27" y="7" text-anchor="end" dy="0.35em" id="img-91c8bd3d-278" gadfly:scale="10.0" visibility="hidden">15</text>
    <text x="16.27" y="4.13" text-anchor="end" dy="0.35em" id="img-91c8bd3d-279" gadfly:scale="10.0" visibility="hidden">16</text>
    <text x="16.27" y="1.26" text-anchor="end" dy="0.35em" id="img-91c8bd3d-280" gadfly:scale="10.0" visibility="hidden">17</text>
    <text x="16.27" y="-1.61" text-anchor="end" dy="0.35em" id="img-91c8bd3d-281" gadfly:scale="10.0" visibility="hidden">18</text>
    <text x="16.27" y="-4.47" text-anchor="end" dy="0.35em" id="img-91c8bd3d-282" gadfly:scale="10.0" visibility="hidden">19</text>
    <text x="16.27" y="-7.34" text-anchor="end" dy="0.35em" id="img-91c8bd3d-283" gadfly:scale="10.0" visibility="hidden">20</text>
    <text x="16.27" y="-10.21" text-anchor="end" dy="0.35em" id="img-91c8bd3d-284" gadfly:scale="10.0" visibility="hidden">21</text>
    <text x="16.27" y="-13.08" text-anchor="end" dy="0.35em" id="img-91c8bd3d-285" gadfly:scale="10.0" visibility="hidden">22</text>
    <text x="16.27" y="-15.95" text-anchor="end" dy="0.35em" id="img-91c8bd3d-286" gadfly:scale="10.0" visibility="hidden">23</text>
    <text x="16.27" y="-18.82" text-anchor="end" dy="0.35em" id="img-91c8bd3d-287" gadfly:scale="10.0" visibility="hidden">24</text>
    <text x="16.27" y="-21.69" text-anchor="end" dy="0.35em" id="img-91c8bd3d-288" gadfly:scale="10.0" visibility="hidden">25</text>
    <text x="16.27" y="-24.55" text-anchor="end" dy="0.35em" id="img-91c8bd3d-289" gadfly:scale="10.0" visibility="hidden">26</text>
    <text x="16.27" y="-27.42" text-anchor="end" dy="0.35em" id="img-91c8bd3d-290" gadfly:scale="10.0" visibility="hidden">27</text>
    <text x="16.27" y="-30.29" text-anchor="end" dy="0.35em" id="img-91c8bd3d-291" gadfly:scale="10.0" visibility="hidden">28</text>
    <text x="16.27" y="-33.16" text-anchor="end" dy="0.35em" id="img-91c8bd3d-292" gadfly:scale="10.0" visibility="hidden">29</text>
    <text x="16.27" y="-36.03" text-anchor="end" dy="0.35em" id="img-91c8bd3d-293" gadfly:scale="10.0" visibility="hidden">30</text>
    <text x="16.27" y="-38.9" text-anchor="end" dy="0.35em" id="img-91c8bd3d-294" gadfly:scale="10.0" visibility="hidden">31</text>
    <text x="16.27" y="-41.77" text-anchor="end" dy="0.35em" id="img-91c8bd3d-295" gadfly:scale="10.0" visibility="hidden">32</text>
    <text x="16.27" y="-44.63" text-anchor="end" dy="0.35em" id="img-91c8bd3d-296" gadfly:scale="10.0" visibility="hidden">33</text>
    <text x="16.27" y="-47.5" text-anchor="end" dy="0.35em" id="img-91c8bd3d-297" gadfly:scale="10.0" visibility="hidden">34</text>
    <text x="16.27" y="-50.37" text-anchor="end" dy="0.35em" id="img-91c8bd3d-298" gadfly:scale="10.0" visibility="hidden">35</text>
    <text x="16.27" y="-53.24" text-anchor="end" dy="0.35em" id="img-91c8bd3d-299" gadfly:scale="10.0" visibility="hidden">36</text>
    <text x="16.27" y="-56.11" text-anchor="end" dy="0.35em" id="img-91c8bd3d-300" gadfly:scale="10.0" visibility="hidden">37</text>
    <text x="16.27" y="-58.98" text-anchor="end" dy="0.35em" id="img-91c8bd3d-301" gadfly:scale="10.0" visibility="hidden">38</text>
    <text x="16.27" y="-61.85" text-anchor="end" dy="0.35em" id="img-91c8bd3d-302" gadfly:scale="10.0" visibility="hidden">39</text>
    <text x="16.27" y="-64.71" text-anchor="end" dy="0.35em" id="img-91c8bd3d-303" gadfly:scale="10.0" visibility="hidden">40</text>
    <text x="16.27" y="164.77" text-anchor="end" dy="0.35em" id="img-91c8bd3d-304" gadfly:scale="0.5" visibility="hidden">-40</text>
    <text x="16.27" y="107.4" text-anchor="end" dy="0.35em" id="img-91c8bd3d-305" gadfly:scale="0.5" visibility="hidden">-20</text>
    <text x="16.27" y="50.03" text-anchor="end" dy="0.35em" id="img-91c8bd3d-306" gadfly:scale="0.5" visibility="hidden">0</text>
    <text x="16.27" y="-7.34" text-anchor="end" dy="0.35em" id="img-91c8bd3d-307" gadfly:scale="0.5" visibility="hidden">20</text>
    <text x="16.27" y="-64.71" text-anchor="end" dy="0.35em" id="img-91c8bd3d-308" gadfly:scale="0.5" visibility="hidden">40</text>
    <text x="16.27" y="153.3" text-anchor="end" dy="0.35em" id="img-91c8bd3d-309" gadfly:scale="5.0" visibility="hidden">-36</text>
    <text x="16.27" y="147.56" text-anchor="end" dy="0.35em" id="img-91c8bd3d-310" gadfly:scale="5.0" visibility="hidden">-34</text>
    <text x="16.27" y="141.82" text-anchor="end" dy="0.35em" id="img-91c8bd3d-311" gadfly:scale="5.0" visibility="hidden">-32</text>
    <text x="16.27" y="136.09" text-anchor="end" dy="0.35em" id="img-91c8bd3d-312" gadfly:scale="5.0" visibility="hidden">-30</text>
    <text x="16.27" y="130.35" text-anchor="end" dy="0.35em" id="img-91c8bd3d-313" gadfly:scale="5.0" visibility="hidden">-28</text>
    <text x="16.27" y="124.61" text-anchor="end" dy="0.35em" id="img-91c8bd3d-314" gadfly:scale="5.0" visibility="hidden">-26</text>
    <text x="16.27" y="118.88" text-anchor="end" dy="0.35em" id="img-91c8bd3d-315" gadfly:scale="5.0" visibility="hidden">-24</text>
    <text x="16.27" y="113.14" text-anchor="end" dy="0.35em" id="img-91c8bd3d-316" gadfly:scale="5.0" visibility="hidden">-22</text>
    <text x="16.27" y="107.4" text-anchor="end" dy="0.35em" id="img-91c8bd3d-317" gadfly:scale="5.0" visibility="hidden">-20</text>
    <text x="16.27" y="101.66" text-anchor="end" dy="0.35em" id="img-91c8bd3d-318" gadfly:scale="5.0" visibility="hidden">-18</text>
    <text x="16.27" y="95.93" text-anchor="end" dy="0.35em" id="img-91c8bd3d-319" gadfly:scale="5.0" visibility="hidden">-16</text>
    <text x="16.27" y="90.19" text-anchor="end" dy="0.35em" id="img-91c8bd3d-320" gadfly:scale="5.0" visibility="hidden">-14</text>
    <text x="16.27" y="84.45" text-anchor="end" dy="0.35em" id="img-91c8bd3d-321" gadfly:scale="5.0" visibility="hidden">-12</text>
    <text x="16.27" y="78.72" text-anchor="end" dy="0.35em" id="img-91c8bd3d-322" gadfly:scale="5.0" visibility="hidden">-10</text>
    <text x="16.27" y="72.98" text-anchor="end" dy="0.35em" id="img-91c8bd3d-323" gadfly:scale="5.0" visibility="hidden">-8</text>
    <text x="16.27" y="67.24" text-anchor="end" dy="0.35em" id="img-91c8bd3d-324" gadfly:scale="5.0" visibility="hidden">-6</text>
    <text x="16.27" y="61.5" text-anchor="end" dy="0.35em" id="img-91c8bd3d-325" gadfly:scale="5.0" visibility="hidden">-4</text>
    <text x="16.27" y="55.77" text-anchor="end" dy="0.35em" id="img-91c8bd3d-326" gadfly:scale="5.0" visibility="hidden">-2</text>
    <text x="16.27" y="50.03" text-anchor="end" dy="0.35em" id="img-91c8bd3d-327" gadfly:scale="5.0" visibility="hidden">0</text>
    <text x="16.27" y="44.29" text-anchor="end" dy="0.35em" id="img-91c8bd3d-328" gadfly:scale="5.0" visibility="hidden">2</text>
    <text x="16.27" y="38.55" text-anchor="end" dy="0.35em" id="img-91c8bd3d-329" gadfly:scale="5.0" visibility="hidden">4</text>
    <text x="16.27" y="32.82" text-anchor="end" dy="0.35em" id="img-91c8bd3d-330" gadfly:scale="5.0" visibility="hidden">6</text>
    <text x="16.27" y="27.08" text-anchor="end" dy="0.35em" id="img-91c8bd3d-331" gadfly:scale="5.0" visibility="hidden">8</text>
    <text x="16.27" y="21.34" text-anchor="end" dy="0.35em" id="img-91c8bd3d-332" gadfly:scale="5.0" visibility="hidden">10</text>
    <text x="16.27" y="15.61" text-anchor="end" dy="0.35em" id="img-91c8bd3d-333" gadfly:scale="5.0" visibility="hidden">12</text>
    <text x="16.27" y="9.87" text-anchor="end" dy="0.35em" id="img-91c8bd3d-334" gadfly:scale="5.0" visibility="hidden">14</text>
    <text x="16.27" y="4.13" text-anchor="end" dy="0.35em" id="img-91c8bd3d-335" gadfly:scale="5.0" visibility="hidden">16</text>
    <text x="16.27" y="-1.61" text-anchor="end" dy="0.35em" id="img-91c8bd3d-336" gadfly:scale="5.0" visibility="hidden">18</text>
    <text x="16.27" y="-7.34" text-anchor="end" dy="0.35em" id="img-91c8bd3d-337" gadfly:scale="5.0" visibility="hidden">20</text>
    <text x="16.27" y="-13.08" text-anchor="end" dy="0.35em" id="img-91c8bd3d-338" gadfly:scale="5.0" visibility="hidden">22</text>
    <text x="16.27" y="-18.82" text-anchor="end" dy="0.35em" id="img-91c8bd3d-339" gadfly:scale="5.0" visibility="hidden">24</text>
    <text x="16.27" y="-24.55" text-anchor="end" dy="0.35em" id="img-91c8bd3d-340" gadfly:scale="5.0" visibility="hidden">26</text>
    <text x="16.27" y="-30.29" text-anchor="end" dy="0.35em" id="img-91c8bd3d-341" gadfly:scale="5.0" visibility="hidden">28</text>
    <text x="16.27" y="-36.03" text-anchor="end" dy="0.35em" id="img-91c8bd3d-342" gadfly:scale="5.0" visibility="hidden">30</text>
    <text x="16.27" y="-41.77" text-anchor="end" dy="0.35em" id="img-91c8bd3d-343" gadfly:scale="5.0" visibility="hidden">32</text>
    <text x="16.27" y="-47.5" text-anchor="end" dy="0.35em" id="img-91c8bd3d-344" gadfly:scale="5.0" visibility="hidden">34</text>
    <text x="16.27" y="-53.24" text-anchor="end" dy="0.35em" id="img-91c8bd3d-345" gadfly:scale="5.0" visibility="hidden">36</text>
    <text x="16.27" y="-58.98" text-anchor="end" dy="0.35em" id="img-91c8bd3d-346" gadfly:scale="5.0" visibility="hidden">38</text>
    <text x="16.27" y="-64.71" text-anchor="end" dy="0.35em" id="img-91c8bd3d-347" gadfly:scale="5.0" visibility="hidden">40</text>
  </g>
  <g font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" fill="#564A55" stroke="#000000" stroke-opacity="0.000" id="img-91c8bd3d-348">
    <text x="8.81" y="42.86" text-anchor="end" dy="0.35em" id="img-91c8bd3d-349">y</text>
  </g>
</g>
<defs>
  <clipPath id="img-91c8bd3d-15">
  <path d="M17.27,5 L 117.45 5 117.45 80.72 17.27 80.72" />
</clipPath>
</defs>
<script> <![CDATA[
(function(N){var k=/[\.\/]/,L=/\s*,\s*/,C=function(a,d){return a-d},a,v,y={n:{}},M=function(){for(var a=0,d=this.length;a<d;a++)if("undefined"!=typeof this[a])return this[a]},A=function(){for(var a=this.length;--a;)if("undefined"!=typeof this[a])return this[a]},w=function(k,d){k=String(k);var f=v,n=Array.prototype.slice.call(arguments,2),u=w.listeners(k),p=0,b,q=[],e={},l=[],r=a;l.firstDefined=M;l.lastDefined=A;a=k;for(var s=v=0,x=u.length;s<x;s++)"zIndex"in u[s]&&(q.push(u[s].zIndex),0>u[s].zIndex&&
(e[u[s].zIndex]=u[s]));for(q.sort(C);0>q[p];)if(b=e[q[p++] ],l.push(b.apply(d,n)),v)return v=f,l;for(s=0;s<x;s++)if(b=u[s],"zIndex"in b)if(b.zIndex==q[p]){l.push(b.apply(d,n));if(v)break;do if(p++,(b=e[q[p] ])&&l.push(b.apply(d,n)),v)break;while(b)}else e[b.zIndex]=b;else if(l.push(b.apply(d,n)),v)break;v=f;a=r;return l};w._events=y;w.listeners=function(a){a=a.split(k);var d=y,f,n,u,p,b,q,e,l=[d],r=[];u=0;for(p=a.length;u<p;u++){e=[];b=0;for(q=l.length;b<q;b++)for(d=l[b].n,f=[d[a[u] ],d["*"] ],n=2;n--;)if(d=
f[n])e.push(d),r=r.concat(d.f||[]);l=e}return r};w.on=function(a,d){a=String(a);if("function"!=typeof d)return function(){};for(var f=a.split(L),n=0,u=f.length;n<u;n++)(function(a){a=a.split(k);for(var b=y,f,e=0,l=a.length;e<l;e++)b=b.n,b=b.hasOwnProperty(a[e])&&b[a[e] ]||(b[a[e] ]={n:{}});b.f=b.f||[];e=0;for(l=b.f.length;e<l;e++)if(b.f[e]==d){f=!0;break}!f&&b.f.push(d)})(f[n]);return function(a){+a==+a&&(d.zIndex=+a)}};w.f=function(a){var d=[].slice.call(arguments,1);return function(){w.apply(null,
[a,null].concat(d).concat([].slice.call(arguments,0)))}};w.stop=function(){v=1};w.nt=function(k){return k?(new RegExp("(?:\\.|\\/|^)"+k+"(?:\\.|\\/|$)")).test(a):a};w.nts=function(){return a.split(k)};w.off=w.unbind=function(a,d){if(a){var f=a.split(L);if(1<f.length)for(var n=0,u=f.length;n<u;n++)w.off(f[n],d);else{for(var f=a.split(k),p,b,q,e,l=[y],n=0,u=f.length;n<u;n++)for(e=0;e<l.length;e+=q.length-2){q=[e,1];p=l[e].n;if("*"!=f[n])p[f[n] ]&&q.push(p[f[n] ]);else for(b in p)p.hasOwnProperty(b)&&
q.push(p[b]);l.splice.apply(l,q)}n=0;for(u=l.length;n<u;n++)for(p=l[n];p.n;){if(d){if(p.f){e=0;for(f=p.f.length;e<f;e++)if(p.f[e]==d){p.f.splice(e,1);break}!p.f.length&&delete p.f}for(b in p.n)if(p.n.hasOwnProperty(b)&&p.n[b].f){q=p.n[b].f;e=0;for(f=q.length;e<f;e++)if(q[e]==d){q.splice(e,1);break}!q.length&&delete p.n[b].f}}else for(b in delete p.f,p.n)p.n.hasOwnProperty(b)&&p.n[b].f&&delete p.n[b].f;p=p.n}}}else w._events=y={n:{}}};w.once=function(a,d){var f=function(){w.unbind(a,f);return d.apply(this,
arguments)};return w.on(a,f)};w.version="0.4.2";w.toString=function(){return"You are running Eve 0.4.2"};"undefined"!=typeof module&&module.exports?module.exports=w:"function"===typeof define&&define.amd?define("eve",[],function(){return w}):N.eve=w})(this);
(function(N,k){"function"===typeof define&&define.amd?define("Snap.svg",["eve"],function(L){return k(N,L)}):k(N,N.eve)})(this,function(N,k){var L=function(a){var k={},y=N.requestAnimationFrame||N.webkitRequestAnimationFrame||N.mozRequestAnimationFrame||N.oRequestAnimationFrame||N.msRequestAnimationFrame||function(a){setTimeout(a,16)},M=Array.isArray||function(a){return a instanceof Array||"[object Array]"==Object.prototype.toString.call(a)},A=0,w="M"+(+new Date).toString(36),z=function(a){if(null==
a)return this.s;var b=this.s-a;this.b+=this.dur*b;this.B+=this.dur*b;this.s=a},d=function(a){if(null==a)return this.spd;this.spd=a},f=function(a){if(null==a)return this.dur;this.s=this.s*a/this.dur;this.dur=a},n=function(){delete k[this.id];this.update();a("mina.stop."+this.id,this)},u=function(){this.pdif||(delete k[this.id],this.update(),this.pdif=this.get()-this.b)},p=function(){this.pdif&&(this.b=this.get()-this.pdif,delete this.pdif,k[this.id]=this)},b=function(){var a;if(M(this.start)){a=[];
for(var b=0,e=this.start.length;b<e;b++)a[b]=+this.start[b]+(this.end[b]-this.start[b])*this.easing(this.s)}else a=+this.start+(this.end-this.start)*this.easing(this.s);this.set(a)},q=function(){var l=0,b;for(b in k)if(k.hasOwnProperty(b)){var e=k[b],f=e.get();l++;e.s=(f-e.b)/(e.dur/e.spd);1<=e.s&&(delete k[b],e.s=1,l--,function(b){setTimeout(function(){a("mina.finish."+b.id,b)})}(e));e.update()}l&&y(q)},e=function(a,r,s,x,G,h,J){a={id:w+(A++).toString(36),start:a,end:r,b:s,s:0,dur:x-s,spd:1,get:G,
set:h,easing:J||e.linear,status:z,speed:d,duration:f,stop:n,pause:u,resume:p,update:b};k[a.id]=a;r=0;for(var K in k)if(k.hasOwnProperty(K)&&(r++,2==r))break;1==r&&y(q);return a};e.time=Date.now||function(){return+new Date};e.getById=function(a){return k[a]||null};e.linear=function(a){return a};e.easeout=function(a){return Math.pow(a,1.7)};e.easein=function(a){return Math.pow(a,0.48)};e.easeinout=function(a){if(1==a)return 1;if(0==a)return 0;var b=0.48-a/1.04,e=Math.sqrt(0.1734+b*b);a=e-b;a=Math.pow(Math.abs(a),
1/3)*(0>a?-1:1);b=-e-b;b=Math.pow(Math.abs(b),1/3)*(0>b?-1:1);a=a+b+0.5;return 3*(1-a)*a*a+a*a*a};e.backin=function(a){return 1==a?1:a*a*(2.70158*a-1.70158)};e.backout=function(a){if(0==a)return 0;a-=1;return a*a*(2.70158*a+1.70158)+1};e.elastic=function(a){return a==!!a?a:Math.pow(2,-10*a)*Math.sin(2*(a-0.075)*Math.PI/0.3)+1};e.bounce=function(a){a<1/2.75?a*=7.5625*a:a<2/2.75?(a-=1.5/2.75,a=7.5625*a*a+0.75):a<2.5/2.75?(a-=2.25/2.75,a=7.5625*a*a+0.9375):(a-=2.625/2.75,a=7.5625*a*a+0.984375);return a};
return N.mina=e}("undefined"==typeof k?function(){}:k),C=function(){function a(c,t){if(c){if(c.tagName)return x(c);if(y(c,"array")&&a.set)return a.set.apply(a,c);if(c instanceof e)return c;if(null==t)return c=G.doc.querySelector(c),x(c)}return new s(null==c?"100%":c,null==t?"100%":t)}function v(c,a){if(a){"#text"==c&&(c=G.doc.createTextNode(a.text||""));"string"==typeof c&&(c=v(c));if("string"==typeof a)return"xlink:"==a.substring(0,6)?c.getAttributeNS(m,a.substring(6)):"xml:"==a.substring(0,4)?c.getAttributeNS(la,
a.substring(4)):c.getAttribute(a);for(var da in a)if(a[h](da)){var b=J(a[da]);b?"xlink:"==da.substring(0,6)?c.setAttributeNS(m,da.substring(6),b):"xml:"==da.substring(0,4)?c.setAttributeNS(la,da.substring(4),b):c.setAttribute(da,b):c.removeAttribute(da)}}else c=G.doc.createElementNS(la,c);return c}function y(c,a){a=J.prototype.toLowerCase.call(a);return"finite"==a?isFinite(c):"array"==a&&(c instanceof Array||Array.isArray&&Array.isArray(c))?!0:"null"==a&&null===c||a==typeof c&&null!==c||"object"==
a&&c===Object(c)||$.call(c).slice(8,-1).toLowerCase()==a}function M(c){if("function"==typeof c||Object(c)!==c)return c;var a=new c.constructor,b;for(b in c)c[h](b)&&(a[b]=M(c[b]));return a}function A(c,a,b){function m(){var e=Array.prototype.slice.call(arguments,0),f=e.join("\u2400"),d=m.cache=m.cache||{},l=m.count=m.count||[];if(d[h](f)){a:for(var e=l,l=f,B=0,H=e.length;B<H;B++)if(e[B]===l){e.push(e.splice(B,1)[0]);break a}return b?b(d[f]):d[f]}1E3<=l.length&&delete d[l.shift()];l.push(f);d[f]=c.apply(a,
e);return b?b(d[f]):d[f]}return m}function w(c,a,b,m,e,f){return null==e?(c-=b,a-=m,c||a?(180*I.atan2(-a,-c)/C+540)%360:0):w(c,a,e,f)-w(b,m,e,f)}function z(c){return c%360*C/180}function d(c){var a=[];c=c.replace(/(?:^|\s)(\w+)\(([^)]+)\)/g,function(c,b,m){m=m.split(/\s*,\s*|\s+/);"rotate"==b&&1==m.length&&m.push(0,0);"scale"==b&&(2<m.length?m=m.slice(0,2):2==m.length&&m.push(0,0),1==m.length&&m.push(m[0],0,0));"skewX"==b?a.push(["m",1,0,I.tan(z(m[0])),1,0,0]):"skewY"==b?a.push(["m",1,I.tan(z(m[0])),
0,1,0,0]):a.push([b.charAt(0)].concat(m));return c});return a}function f(c,t){var b=O(c),m=new a.Matrix;if(b)for(var e=0,f=b.length;e<f;e++){var h=b[e],d=h.length,B=J(h[0]).toLowerCase(),H=h[0]!=B,l=H?m.invert():0,E;"t"==B&&2==d?m.translate(h[1],0):"t"==B&&3==d?H?(d=l.x(0,0),B=l.y(0,0),H=l.x(h[1],h[2]),l=l.y(h[1],h[2]),m.translate(H-d,l-B)):m.translate(h[1],h[2]):"r"==B?2==d?(E=E||t,m.rotate(h[1],E.x+E.width/2,E.y+E.height/2)):4==d&&(H?(H=l.x(h[2],h[3]),l=l.y(h[2],h[3]),m.rotate(h[1],H,l)):m.rotate(h[1],
h[2],h[3])):"s"==B?2==d||3==d?(E=E||t,m.scale(h[1],h[d-1],E.x+E.width/2,E.y+E.height/2)):4==d?H?(H=l.x(h[2],h[3]),l=l.y(h[2],h[3]),m.scale(h[1],h[1],H,l)):m.scale(h[1],h[1],h[2],h[3]):5==d&&(H?(H=l.x(h[3],h[4]),l=l.y(h[3],h[4]),m.scale(h[1],h[2],H,l)):m.scale(h[1],h[2],h[3],h[4])):"m"==B&&7==d&&m.add(h[1],h[2],h[3],h[4],h[5],h[6])}return m}function n(c,t){if(null==t){var m=!0;t="linearGradient"==c.type||"radialGradient"==c.type?c.node.getAttribute("gradientTransform"):"pattern"==c.type?c.node.getAttribute("patternTransform"):
c.node.getAttribute("transform");if(!t)return new a.Matrix;t=d(t)}else t=a._.rgTransform.test(t)?J(t).replace(/\.{3}|\u2026/g,c._.transform||aa):d(t),y(t,"array")&&(t=a.path?a.path.toString.call(t):J(t)),c._.transform=t;var b=f(t,c.getBBox(1));if(m)return b;c.matrix=b}function u(c){c=c.node.ownerSVGElement&&x(c.node.ownerSVGElement)||c.node.parentNode&&x(c.node.parentNode)||a.select("svg")||a(0,0);var t=c.select("defs"),t=null==t?!1:t.node;t||(t=r("defs",c.node).node);return t}function p(c){return c.node.ownerSVGElement&&
x(c.node.ownerSVGElement)||a.select("svg")}function b(c,a,m){function b(c){if(null==c)return aa;if(c==+c)return c;v(B,{width:c});try{return B.getBBox().width}catch(a){return 0}}function h(c){if(null==c)return aa;if(c==+c)return c;v(B,{height:c});try{return B.getBBox().height}catch(a){return 0}}function e(b,B){null==a?d[b]=B(c.attr(b)||0):b==a&&(d=B(null==m?c.attr(b)||0:m))}var f=p(c).node,d={},B=f.querySelector(".svg---mgr");B||(B=v("rect"),v(B,{x:-9E9,y:-9E9,width:10,height:10,"class":"svg---mgr",
fill:"none"}),f.appendChild(B));switch(c.type){case "rect":e("rx",b),e("ry",h);case "image":e("width",b),e("height",h);case "text":e("x",b);e("y",h);break;case "circle":e("cx",b);e("cy",h);e("r",b);break;case "ellipse":e("cx",b);e("cy",h);e("rx",b);e("ry",h);break;case "line":e("x1",b);e("x2",b);e("y1",h);e("y2",h);break;case "marker":e("refX",b);e("markerWidth",b);e("refY",h);e("markerHeight",h);break;case "radialGradient":e("fx",b);e("fy",h);break;case "tspan":e("dx",b);e("dy",h);break;default:e(a,
b)}f.removeChild(B);return d}function q(c){y(c,"array")||(c=Array.prototype.slice.call(arguments,0));for(var a=0,b=0,m=this.node;this[a];)delete this[a++];for(a=0;a<c.length;a++)"set"==c[a].type?c[a].forEach(function(c){m.appendChild(c.node)}):m.appendChild(c[a].node);for(var h=m.childNodes,a=0;a<h.length;a++)this[b++]=x(h[a]);return this}function e(c){if(c.snap in E)return E[c.snap];var a=this.id=V(),b;try{b=c.ownerSVGElement}catch(m){}this.node=c;b&&(this.paper=new s(b));this.type=c.tagName;this.anims=
{};this._={transform:[]};c.snap=a;E[a]=this;"g"==this.type&&(this.add=q);if(this.type in{g:1,mask:1,pattern:1})for(var e in s.prototype)s.prototype[h](e)&&(this[e]=s.prototype[e])}function l(c){this.node=c}function r(c,a){var b=v(c);a.appendChild(b);return x(b)}function s(c,a){var b,m,f,d=s.prototype;if(c&&"svg"==c.tagName){if(c.snap in E)return E[c.snap];var l=c.ownerDocument;b=new e(c);m=c.getElementsByTagName("desc")[0];f=c.getElementsByTagName("defs")[0];m||(m=v("desc"),m.appendChild(l.createTextNode("Created with Snap")),
b.node.appendChild(m));f||(f=v("defs"),b.node.appendChild(f));b.defs=f;for(var ca in d)d[h](ca)&&(b[ca]=d[ca]);b.paper=b.root=b}else b=r("svg",G.doc.body),v(b.node,{height:a,version:1.1,width:c,xmlns:la});return b}function x(c){return!c||c instanceof e||c instanceof l?c:c.tagName&&"svg"==c.tagName.toLowerCase()?new s(c):c.tagName&&"object"==c.tagName.toLowerCase()&&"image/svg+xml"==c.type?new s(c.contentDocument.getElementsByTagName("svg")[0]):new e(c)}a.version="0.3.0";a.toString=function(){return"Snap v"+
this.version};a._={};var G={win:N,doc:N.document};a._.glob=G;var h="hasOwnProperty",J=String,K=parseFloat,U=parseInt,I=Math,P=I.max,Q=I.min,Y=I.abs,C=I.PI,aa="",$=Object.prototype.toString,F=/^\s*((#[a-f\d]{6})|(#[a-f\d]{3})|rgba?\(\s*([\d\.]+%?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+%?(?:\s*,\s*[\d\.]+%?)?)\s*\)|hsba?\(\s*([\d\.]+(?:deg|\xb0|%)?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+(?:%?\s*,\s*[\d\.]+)?%?)\s*\)|hsla?\(\s*([\d\.]+(?:deg|\xb0|%)?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+(?:%?\s*,\s*[\d\.]+)?%?)\s*\))\s*$/i;a._.separator=
RegExp("[,\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]+");var S=RegExp("[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*"),X={hs:1,rg:1},W=RegExp("([a-z])[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029,]*((-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*)+)",
"ig"),ma=RegExp("([rstm])[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029,]*((-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*)+)","ig"),Z=RegExp("(-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?)[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*",
"ig"),na=0,ba="S"+(+new Date).toString(36),V=function(){return ba+(na++).toString(36)},m="http://www.w3.org/1999/xlink",la="http://www.w3.org/2000/svg",E={},ca=a.url=function(c){return"url('#"+c+"')"};a._.$=v;a._.id=V;a.format=function(){var c=/\{([^\}]+)\}/g,a=/(?:(?:^|\.)(.+?)(?=\[|\.|$|\()|\[('|")(.+?)\2\])(\(\))?/g,b=function(c,b,m){var h=m;b.replace(a,function(c,a,b,m,t){a=a||m;h&&(a in h&&(h=h[a]),"function"==typeof h&&t&&(h=h()))});return h=(null==h||h==m?c:h)+""};return function(a,m){return J(a).replace(c,
function(c,a){return b(c,a,m)})}}();a._.clone=M;a._.cacher=A;a.rad=z;a.deg=function(c){return 180*c/C%360};a.angle=w;a.is=y;a.snapTo=function(c,a,b){b=y(b,"finite")?b:10;if(y(c,"array"))for(var m=c.length;m--;){if(Y(c[m]-a)<=b)return c[m]}else{c=+c;m=a%c;if(m<b)return a-m;if(m>c-b)return a-m+c}return a};a.getRGB=A(function(c){if(!c||(c=J(c)).indexOf("-")+1)return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka};if("none"==c)return{r:-1,g:-1,b:-1,hex:"none",toString:ka};!X[h](c.toLowerCase().substring(0,
2))&&"#"!=c.charAt()&&(c=T(c));if(!c)return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka};var b,m,e,f,d;if(c=c.match(F)){c[2]&&(e=U(c[2].substring(5),16),m=U(c[2].substring(3,5),16),b=U(c[2].substring(1,3),16));c[3]&&(e=U((d=c[3].charAt(3))+d,16),m=U((d=c[3].charAt(2))+d,16),b=U((d=c[3].charAt(1))+d,16));c[4]&&(d=c[4].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b*=2.55),m=K(d[1]),"%"==d[1].slice(-1)&&(m*=2.55),e=K(d[2]),"%"==d[2].slice(-1)&&(e*=2.55),"rgba"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),
d[3]&&"%"==d[3].slice(-1)&&(f/=100));if(c[5])return d=c[5].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b/=100),m=K(d[1]),"%"==d[1].slice(-1)&&(m/=100),e=K(d[2]),"%"==d[2].slice(-1)&&(e/=100),"deg"!=d[0].slice(-3)&&"\u00b0"!=d[0].slice(-1)||(b/=360),"hsba"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),d[3]&&"%"==d[3].slice(-1)&&(f/=100),a.hsb2rgb(b,m,e,f);if(c[6])return d=c[6].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b/=100),m=K(d[1]),"%"==d[1].slice(-1)&&(m/=100),e=K(d[2]),"%"==d[2].slice(-1)&&(e/=100),
"deg"!=d[0].slice(-3)&&"\u00b0"!=d[0].slice(-1)||(b/=360),"hsla"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),d[3]&&"%"==d[3].slice(-1)&&(f/=100),a.hsl2rgb(b,m,e,f);b=Q(I.round(b),255);m=Q(I.round(m),255);e=Q(I.round(e),255);f=Q(P(f,0),1);c={r:b,g:m,b:e,toString:ka};c.hex="#"+(16777216|e|m<<8|b<<16).toString(16).slice(1);c.opacity=y(f,"finite")?f:1;return c}return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka}},a);a.hsb=A(function(c,b,m){return a.hsb2rgb(c,b,m).hex});a.hsl=A(function(c,b,m){return a.hsl2rgb(c,
b,m).hex});a.rgb=A(function(c,a,b,m){if(y(m,"finite")){var e=I.round;return"rgba("+[e(c),e(a),e(b),+m.toFixed(2)]+")"}return"#"+(16777216|b|a<<8|c<<16).toString(16).slice(1)});var T=function(c){var a=G.doc.getElementsByTagName("head")[0]||G.doc.getElementsByTagName("svg")[0];T=A(function(c){if("red"==c.toLowerCase())return"rgb(255, 0, 0)";a.style.color="rgb(255, 0, 0)";a.style.color=c;c=G.doc.defaultView.getComputedStyle(a,aa).getPropertyValue("color");return"rgb(255, 0, 0)"==c?null:c});return T(c)},
qa=function(){return"hsb("+[this.h,this.s,this.b]+")"},ra=function(){return"hsl("+[this.h,this.s,this.l]+")"},ka=function(){return 1==this.opacity||null==this.opacity?this.hex:"rgba("+[this.r,this.g,this.b,this.opacity]+")"},D=function(c,b,m){null==b&&y(c,"object")&&"r"in c&&"g"in c&&"b"in c&&(m=c.b,b=c.g,c=c.r);null==b&&y(c,string)&&(m=a.getRGB(c),c=m.r,b=m.g,m=m.b);if(1<c||1<b||1<m)c/=255,b/=255,m/=255;return[c,b,m]},oa=function(c,b,m,e){c=I.round(255*c);b=I.round(255*b);m=I.round(255*m);c={r:c,
g:b,b:m,opacity:y(e,"finite")?e:1,hex:a.rgb(c,b,m),toString:ka};y(e,"finite")&&(c.opacity=e);return c};a.color=function(c){var b;y(c,"object")&&"h"in c&&"s"in c&&"b"in c?(b=a.hsb2rgb(c),c.r=b.r,c.g=b.g,c.b=b.b,c.opacity=1,c.hex=b.hex):y(c,"object")&&"h"in c&&"s"in c&&"l"in c?(b=a.hsl2rgb(c),c.r=b.r,c.g=b.g,c.b=b.b,c.opacity=1,c.hex=b.hex):(y(c,"string")&&(c=a.getRGB(c)),y(c,"object")&&"r"in c&&"g"in c&&"b"in c&&!("error"in c)?(b=a.rgb2hsl(c),c.h=b.h,c.s=b.s,c.l=b.l,b=a.rgb2hsb(c),c.v=b.b):(c={hex:"none"},
c.r=c.g=c.b=c.h=c.s=c.v=c.l=-1,c.error=1));c.toString=ka;return c};a.hsb2rgb=function(c,a,b,m){y(c,"object")&&"h"in c&&"s"in c&&"b"in c&&(b=c.b,a=c.s,c=c.h,m=c.o);var e,h,d;c=360*c%360/60;d=b*a;a=d*(1-Y(c%2-1));b=e=h=b-d;c=~~c;b+=[d,a,0,0,a,d][c];e+=[a,d,d,a,0,0][c];h+=[0,0,a,d,d,a][c];return oa(b,e,h,m)};a.hsl2rgb=function(c,a,b,m){y(c,"object")&&"h"in c&&"s"in c&&"l"in c&&(b=c.l,a=c.s,c=c.h);if(1<c||1<a||1<b)c/=360,a/=100,b/=100;var e,h,d;c=360*c%360/60;d=2*a*(0.5>b?b:1-b);a=d*(1-Y(c%2-1));b=e=
h=b-d/2;c=~~c;b+=[d,a,0,0,a,d][c];e+=[a,d,d,a,0,0][c];h+=[0,0,a,d,d,a][c];return oa(b,e,h,m)};a.rgb2hsb=function(c,a,b){b=D(c,a,b);c=b[0];a=b[1];b=b[2];var m,e;m=P(c,a,b);e=m-Q(c,a,b);c=((0==e?0:m==c?(a-b)/e:m==a?(b-c)/e+2:(c-a)/e+4)+360)%6*60/360;return{h:c,s:0==e?0:e/m,b:m,toString:qa}};a.rgb2hsl=function(c,a,b){b=D(c,a,b);c=b[0];a=b[1];b=b[2];var m,e,h;m=P(c,a,b);e=Q(c,a,b);h=m-e;c=((0==h?0:m==c?(a-b)/h:m==a?(b-c)/h+2:(c-a)/h+4)+360)%6*60/360;m=(m+e)/2;return{h:c,s:0==h?0:0.5>m?h/(2*m):h/(2-2*
m),l:m,toString:ra}};a.parsePathString=function(c){if(!c)return null;var b=a.path(c);if(b.arr)return a.path.clone(b.arr);var m={a:7,c:6,o:2,h:1,l:2,m:2,r:4,q:4,s:4,t:2,v:1,u:3,z:0},e=[];y(c,"array")&&y(c[0],"array")&&(e=a.path.clone(c));e.length||J(c).replace(W,function(c,a,b){var h=[];c=a.toLowerCase();b.replace(Z,function(c,a){a&&h.push(+a)});"m"==c&&2<h.length&&(e.push([a].concat(h.splice(0,2))),c="l",a="m"==a?"l":"L");"o"==c&&1==h.length&&e.push([a,h[0] ]);if("r"==c)e.push([a].concat(h));else for(;h.length>=
m[c]&&(e.push([a].concat(h.splice(0,m[c]))),m[c]););});e.toString=a.path.toString;b.arr=a.path.clone(e);return e};var O=a.parseTransformString=function(c){if(!c)return null;var b=[];y(c,"array")&&y(c[0],"array")&&(b=a.path.clone(c));b.length||J(c).replace(ma,function(c,a,m){var e=[];a.toLowerCase();m.replace(Z,function(c,a){a&&e.push(+a)});b.push([a].concat(e))});b.toString=a.path.toString;return b};a._.svgTransform2string=d;a._.rgTransform=RegExp("^[a-z][\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*-?\\.?\\d",
"i");a._.transform2matrix=f;a._unit2px=b;a._.getSomeDefs=u;a._.getSomeSVG=p;a.select=function(c){return x(G.doc.querySelector(c))};a.selectAll=function(c){c=G.doc.querySelectorAll(c);for(var b=(a.set||Array)(),m=0;m<c.length;m++)b.push(x(c[m]));return b};setInterval(function(){for(var c in E)if(E[h](c)){var a=E[c],b=a.node;("svg"!=a.type&&!b.ownerSVGElement||"svg"==a.type&&(!b.parentNode||"ownerSVGElement"in b.parentNode&&!b.ownerSVGElement))&&delete E[c]}},1E4);(function(c){function m(c){function a(c,
b){var m=v(c.node,b);(m=(m=m&&m.match(d))&&m[2])&&"#"==m.charAt()&&(m=m.substring(1))&&(f[m]=(f[m]||[]).concat(function(a){var m={};m[b]=ca(a);v(c.node,m)}))}function b(c){var a=v(c.node,"xlink:href");a&&"#"==a.charAt()&&(a=a.substring(1))&&(f[a]=(f[a]||[]).concat(function(a){c.attr("xlink:href","#"+a)}))}var e=c.selectAll("*"),h,d=/^\s*url\(("|'|)(.*)\1\)\s*$/;c=[];for(var f={},l=0,E=e.length;l<E;l++){h=e[l];a(h,"fill");a(h,"stroke");a(h,"filter");a(h,"mask");a(h,"clip-path");b(h);var t=v(h.node,
"id");t&&(v(h.node,{id:h.id}),c.push({old:t,id:h.id}))}l=0;for(E=c.length;l<E;l++)if(e=f[c[l].old])for(h=0,t=e.length;h<t;h++)e[h](c[l].id)}function e(c,a,b){return function(m){m=m.slice(c,a);1==m.length&&(m=m[0]);return b?b(m):m}}function d(c){return function(){var a=c?"<"+this.type:"",b=this.node.attributes,m=this.node.childNodes;if(c)for(var e=0,h=b.length;e<h;e++)a+=" "+b[e].name+'="'+b[e].value.replace(/"/g,'\\"')+'"';if(m.length){c&&(a+=">");e=0;for(h=m.length;e<h;e++)3==m[e].nodeType?a+=m[e].nodeValue:
1==m[e].nodeType&&(a+=x(m[e]).toString());c&&(a+="</"+this.type+">")}else c&&(a+="/>");return a}}c.attr=function(c,a){if(!c)return this;if(y(c,"string"))if(1<arguments.length){var b={};b[c]=a;c=b}else return k("snap.util.getattr."+c,this).firstDefined();for(var m in c)c[h](m)&&k("snap.util.attr."+m,this,c[m]);return this};c.getBBox=function(c){if(!a.Matrix||!a.path)return this.node.getBBox();var b=this,m=new a.Matrix;if(b.removed)return a._.box();for(;"use"==b.type;)if(c||(m=m.add(b.transform().localMatrix.translate(b.attr("x")||
0,b.attr("y")||0))),b.original)b=b.original;else var e=b.attr("xlink:href"),b=b.original=b.node.ownerDocument.getElementById(e.substring(e.indexOf("#")+1));var e=b._,h=a.path.get[b.type]||a.path.get.deflt;try{if(c)return e.bboxwt=h?a.path.getBBox(b.realPath=h(b)):a._.box(b.node.getBBox()),a._.box(e.bboxwt);b.realPath=h(b);b.matrix=b.transform().localMatrix;e.bbox=a.path.getBBox(a.path.map(b.realPath,m.add(b.matrix)));return a._.box(e.bbox)}catch(d){return a._.box()}};var f=function(){return this.string};
c.transform=function(c){var b=this._;if(null==c){var m=this;c=new a.Matrix(this.node.getCTM());for(var e=n(this),h=[e],d=new a.Matrix,l=e.toTransformString(),b=J(e)==J(this.matrix)?J(b.transform):l;"svg"!=m.type&&(m=m.parent());)h.push(n(m));for(m=h.length;m--;)d.add(h[m]);return{string:b,globalMatrix:c,totalMatrix:d,localMatrix:e,diffMatrix:c.clone().add(e.invert()),global:c.toTransformString(),total:d.toTransformString(),local:l,toString:f}}c instanceof a.Matrix?this.matrix=c:n(this,c);this.node&&
("linearGradient"==this.type||"radialGradient"==this.type?v(this.node,{gradientTransform:this.matrix}):"pattern"==this.type?v(this.node,{patternTransform:this.matrix}):v(this.node,{transform:this.matrix}));return this};c.parent=function(){return x(this.node.parentNode)};c.append=c.add=function(c){if(c){if("set"==c.type){var a=this;c.forEach(function(c){a.add(c)});return this}c=x(c);this.node.appendChild(c.node);c.paper=this.paper}return this};c.appendTo=function(c){c&&(c=x(c),c.append(this));return this};
c.prepend=function(c){if(c){if("set"==c.type){var a=this,b;c.forEach(function(c){b?b.after(c):a.prepend(c);b=c});return this}c=x(c);var m=c.parent();this.node.insertBefore(c.node,this.node.firstChild);this.add&&this.add();c.paper=this.paper;this.parent()&&this.parent().add();m&&m.add()}return this};c.prependTo=function(c){c=x(c);c.prepend(this);return this};c.before=function(c){if("set"==c.type){var a=this;c.forEach(function(c){var b=c.parent();a.node.parentNode.insertBefore(c.node,a.node);b&&b.add()});
this.parent().add();return this}c=x(c);var b=c.parent();this.node.parentNode.insertBefore(c.node,this.node);this.parent()&&this.parent().add();b&&b.add();c.paper=this.paper;return this};c.after=function(c){c=x(c);var a=c.parent();this.node.nextSibling?this.node.parentNode.insertBefore(c.node,this.node.nextSibling):this.node.parentNode.appendChild(c.node);this.parent()&&this.parent().add();a&&a.add();c.paper=this.paper;return this};c.insertBefore=function(c){c=x(c);var a=this.parent();c.node.parentNode.insertBefore(this.node,
c.node);this.paper=c.paper;a&&a.add();c.parent()&&c.parent().add();return this};c.insertAfter=function(c){c=x(c);var a=this.parent();c.node.parentNode.insertBefore(this.node,c.node.nextSibling);this.paper=c.paper;a&&a.add();c.parent()&&c.parent().add();return this};c.remove=function(){var c=this.parent();this.node.parentNode&&this.node.parentNode.removeChild(this.node);delete this.paper;this.removed=!0;c&&c.add();return this};c.select=function(c){return x(this.node.querySelector(c))};c.selectAll=
function(c){c=this.node.querySelectorAll(c);for(var b=(a.set||Array)(),m=0;m<c.length;m++)b.push(x(c[m]));return b};c.asPX=function(c,a){null==a&&(a=this.attr(c));return+b(this,c,a)};c.use=function(){var c,a=this.node.id;a||(a=this.id,v(this.node,{id:a}));c="linearGradient"==this.type||"radialGradient"==this.type||"pattern"==this.type?r(this.type,this.node.parentNode):r("use",this.node.parentNode);v(c.node,{"xlink:href":"#"+a});c.original=this;return c};var l=/\S+/g;c.addClass=function(c){var a=(c||
"").match(l)||[];c=this.node;var b=c.className.baseVal,m=b.match(l)||[],e,h,d;if(a.length){for(e=0;d=a[e++];)h=m.indexOf(d),~h||m.push(d);a=m.join(" ");b!=a&&(c.className.baseVal=a)}return this};c.removeClass=function(c){var a=(c||"").match(l)||[];c=this.node;var b=c.className.baseVal,m=b.match(l)||[],e,h;if(m.length){for(e=0;h=a[e++];)h=m.indexOf(h),~h&&m.splice(h,1);a=m.join(" ");b!=a&&(c.className.baseVal=a)}return this};c.hasClass=function(c){return!!~(this.node.className.baseVal.match(l)||[]).indexOf(c)};
c.toggleClass=function(c,a){if(null!=a)return a?this.addClass(c):this.removeClass(c);var b=(c||"").match(l)||[],m=this.node,e=m.className.baseVal,h=e.match(l)||[],d,f,E;for(d=0;E=b[d++];)f=h.indexOf(E),~f?h.splice(f,1):h.push(E);b=h.join(" ");e!=b&&(m.className.baseVal=b);return this};c.clone=function(){var c=x(this.node.cloneNode(!0));v(c.node,"id")&&v(c.node,{id:c.id});m(c);c.insertAfter(this);return c};c.toDefs=function(){u(this).appendChild(this.node);return this};c.pattern=c.toPattern=function(c,
a,b,m){var e=r("pattern",u(this));null==c&&(c=this.getBBox());y(c,"object")&&"x"in c&&(a=c.y,b=c.width,m=c.height,c=c.x);v(e.node,{x:c,y:a,width:b,height:m,patternUnits:"userSpaceOnUse",id:e.id,viewBox:[c,a,b,m].join(" ")});e.node.appendChild(this.node);return e};c.marker=function(c,a,b,m,e,h){var d=r("marker",u(this));null==c&&(c=this.getBBox());y(c,"object")&&"x"in c&&(a=c.y,b=c.width,m=c.height,e=c.refX||c.cx,h=c.refY||c.cy,c=c.x);v(d.node,{viewBox:[c,a,b,m].join(" "),markerWidth:b,markerHeight:m,
orient:"auto",refX:e||0,refY:h||0,id:d.id});d.node.appendChild(this.node);return d};var E=function(c,a,b,m){"function"!=typeof b||b.length||(m=b,b=L.linear);this.attr=c;this.dur=a;b&&(this.easing=b);m&&(this.callback=m)};a._.Animation=E;a.animation=function(c,a,b,m){return new E(c,a,b,m)};c.inAnim=function(){var c=[],a;for(a in this.anims)this.anims[h](a)&&function(a){c.push({anim:new E(a._attrs,a.dur,a.easing,a._callback),mina:a,curStatus:a.status(),status:function(c){return a.status(c)},stop:function(){a.stop()}})}(this.anims[a]);
return c};a.animate=function(c,a,b,m,e,h){"function"!=typeof e||e.length||(h=e,e=L.linear);var d=L.time();c=L(c,a,d,d+m,L.time,b,e);h&&k.once("mina.finish."+c.id,h);return c};c.stop=function(){for(var c=this.inAnim(),a=0,b=c.length;a<b;a++)c[a].stop();return this};c.animate=function(c,a,b,m){"function"!=typeof b||b.length||(m=b,b=L.linear);c instanceof E&&(m=c.callback,b=c.easing,a=b.dur,c=c.attr);var d=[],f=[],l={},t,ca,n,T=this,q;for(q in c)if(c[h](q)){T.equal?(n=T.equal(q,J(c[q])),t=n.from,ca=
n.to,n=n.f):(t=+T.attr(q),ca=+c[q]);var la=y(t,"array")?t.length:1;l[q]=e(d.length,d.length+la,n);d=d.concat(t);f=f.concat(ca)}t=L.time();var p=L(d,f,t,t+a,L.time,function(c){var a={},b;for(b in l)l[h](b)&&(a[b]=l[b](c));T.attr(a)},b);T.anims[p.id]=p;p._attrs=c;p._callback=m;k("snap.animcreated."+T.id,p);k.once("mina.finish."+p.id,function(){delete T.anims[p.id];m&&m.call(T)});k.once("mina.stop."+p.id,function(){delete T.anims[p.id]});return T};var T={};c.data=function(c,b){var m=T[this.id]=T[this.id]||
{};if(0==arguments.length)return k("snap.data.get."+this.id,this,m,null),m;if(1==arguments.length){if(a.is(c,"object")){for(var e in c)c[h](e)&&this.data(e,c[e]);return this}k("snap.data.get."+this.id,this,m[c],c);return m[c]}m[c]=b;k("snap.data.set."+this.id,this,b,c);return this};c.removeData=function(c){null==c?T[this.id]={}:T[this.id]&&delete T[this.id][c];return this};c.outerSVG=c.toString=d(1);c.innerSVG=d()})(e.prototype);a.parse=function(c){var a=G.doc.createDocumentFragment(),b=!0,m=G.doc.createElement("div");
c=J(c);c.match(/^\s*<\s*svg(?:\s|>)/)||(c="<svg>"+c+"</svg>",b=!1);m.innerHTML=c;if(c=m.getElementsByTagName("svg")[0])if(b)a=c;else for(;c.firstChild;)a.appendChild(c.firstChild);m.innerHTML=aa;return new l(a)};l.prototype.select=e.prototype.select;l.prototype.selectAll=e.prototype.selectAll;a.fragment=function(){for(var c=Array.prototype.slice.call(arguments,0),b=G.doc.createDocumentFragment(),m=0,e=c.length;m<e;m++){var h=c[m];h.node&&h.node.nodeType&&b.appendChild(h.node);h.nodeType&&b.appendChild(h);
"string"==typeof h&&b.appendChild(a.parse(h).node)}return new l(b)};a._.make=r;a._.wrap=x;s.prototype.el=function(c,a){var b=r(c,this.node);a&&b.attr(a);return b};k.on("snap.util.getattr",function(){var c=k.nt(),c=c.substring(c.lastIndexOf(".")+1),a=c.replace(/[A-Z]/g,function(c){return"-"+c.toLowerCase()});return pa[h](a)?this.node.ownerDocument.defaultView.getComputedStyle(this.node,null).getPropertyValue(a):v(this.node,c)});var pa={"alignment-baseline":0,"baseline-shift":0,clip:0,"clip-path":0,
"clip-rule":0,color:0,"color-interpolation":0,"color-interpolation-filters":0,"color-profile":0,"color-rendering":0,cursor:0,direction:0,display:0,"dominant-baseline":0,"enable-background":0,fill:0,"fill-opacity":0,"fill-rule":0,filter:0,"flood-color":0,"flood-opacity":0,font:0,"font-family":0,"font-size":0,"font-size-adjust":0,"font-stretch":0,"font-style":0,"font-variant":0,"font-weight":0,"glyph-orientation-horizontal":0,"glyph-orientation-vertical":0,"image-rendering":0,kerning:0,"letter-spacing":0,
"lighting-color":0,marker:0,"marker-end":0,"marker-mid":0,"marker-start":0,mask:0,opacity:0,overflow:0,"pointer-events":0,"shape-rendering":0,"stop-color":0,"stop-opacity":0,stroke:0,"stroke-dasharray":0,"stroke-dashoffset":0,"stroke-linecap":0,"stroke-linejoin":0,"stroke-miterlimit":0,"stroke-opacity":0,"stroke-width":0,"text-anchor":0,"text-decoration":0,"text-rendering":0,"unicode-bidi":0,visibility:0,"word-spacing":0,"writing-mode":0};k.on("snap.util.attr",function(c){var a=k.nt(),b={},a=a.substring(a.lastIndexOf(".")+
1);b[a]=c;var m=a.replace(/-(\w)/gi,function(c,a){return a.toUpperCase()}),a=a.replace(/[A-Z]/g,function(c){return"-"+c.toLowerCase()});pa[h](a)?this.node.style[m]=null==c?aa:c:v(this.node,b)});a.ajax=function(c,a,b,m){var e=new XMLHttpRequest,h=V();if(e){if(y(a,"function"))m=b,b=a,a=null;else if(y(a,"object")){var d=[],f;for(f in a)a.hasOwnProperty(f)&&d.push(encodeURIComponent(f)+"="+encodeURIComponent(a[f]));a=d.join("&")}e.open(a?"POST":"GET",c,!0);a&&(e.setRequestHeader("X-Requested-With","XMLHttpRequest"),
e.setRequestHeader("Content-type","application/x-www-form-urlencoded"));b&&(k.once("snap.ajax."+h+".0",b),k.once("snap.ajax."+h+".200",b),k.once("snap.ajax."+h+".304",b));e.onreadystatechange=function(){4==e.readyState&&k("snap.ajax."+h+"."+e.status,m,e)};if(4==e.readyState)return e;e.send(a);return e}};a.load=function(c,b,m){a.ajax(c,function(c){c=a.parse(c.responseText);m?b.call(m,c):b(c)})};a.getElementByPoint=function(c,a){var b,m,e=G.doc.elementFromPoint(c,a);if(G.win.opera&&"svg"==e.tagName){b=
e;m=b.getBoundingClientRect();b=b.ownerDocument;var h=b.body,d=b.documentElement;b=m.top+(g.win.pageYOffset||d.scrollTop||h.scrollTop)-(d.clientTop||h.clientTop||0);m=m.left+(g.win.pageXOffset||d.scrollLeft||h.scrollLeft)-(d.clientLeft||h.clientLeft||0);h=e.createSVGRect();h.x=c-m;h.y=a-b;h.width=h.height=1;b=e.getIntersectionList(h,null);b.length&&(e=b[b.length-1])}return e?x(e):null};a.plugin=function(c){c(a,e,s,G,l)};return G.win.Snap=a}();C.plugin(function(a,k,y,M,A){function w(a,d,f,b,q,e){null==
d&&"[object SVGMatrix]"==z.call(a)?(this.a=a.a,this.b=a.b,this.c=a.c,this.d=a.d,this.e=a.e,this.f=a.f):null!=a?(this.a=+a,this.b=+d,this.c=+f,this.d=+b,this.e=+q,this.f=+e):(this.a=1,this.c=this.b=0,this.d=1,this.f=this.e=0)}var z=Object.prototype.toString,d=String,f=Math;(function(n){function k(a){return a[0]*a[0]+a[1]*a[1]}function p(a){var d=f.sqrt(k(a));a[0]&&(a[0]/=d);a[1]&&(a[1]/=d)}n.add=function(a,d,e,f,n,p){var k=[[],[],[] ],u=[[this.a,this.c,this.e],[this.b,this.d,this.f],[0,0,1] ];d=[[a,
e,n],[d,f,p],[0,0,1] ];a&&a instanceof w&&(d=[[a.a,a.c,a.e],[a.b,a.d,a.f],[0,0,1] ]);for(a=0;3>a;a++)for(e=0;3>e;e++){for(f=n=0;3>f;f++)n+=u[a][f]*d[f][e];k[a][e]=n}this.a=k[0][0];this.b=k[1][0];this.c=k[0][1];this.d=k[1][1];this.e=k[0][2];this.f=k[1][2];return this};n.invert=function(){var a=this.a*this.d-this.b*this.c;return new w(this.d/a,-this.b/a,-this.c/a,this.a/a,(this.c*this.f-this.d*this.e)/a,(this.b*this.e-this.a*this.f)/a)};n.clone=function(){return new w(this.a,this.b,this.c,this.d,this.e,
this.f)};n.translate=function(a,d){return this.add(1,0,0,1,a,d)};n.scale=function(a,d,e,f){null==d&&(d=a);(e||f)&&this.add(1,0,0,1,e,f);this.add(a,0,0,d,0,0);(e||f)&&this.add(1,0,0,1,-e,-f);return this};n.rotate=function(b,d,e){b=a.rad(b);d=d||0;e=e||0;var l=+f.cos(b).toFixed(9);b=+f.sin(b).toFixed(9);this.add(l,b,-b,l,d,e);return this.add(1,0,0,1,-d,-e)};n.x=function(a,d){return a*this.a+d*this.c+this.e};n.y=function(a,d){return a*this.b+d*this.d+this.f};n.get=function(a){return+this[d.fromCharCode(97+
a)].toFixed(4)};n.toString=function(){return"matrix("+[this.get(0),this.get(1),this.get(2),this.get(3),this.get(4),this.get(5)].join()+")"};n.offset=function(){return[this.e.toFixed(4),this.f.toFixed(4)]};n.determinant=function(){return this.a*this.d-this.b*this.c};n.split=function(){var b={};b.dx=this.e;b.dy=this.f;var d=[[this.a,this.c],[this.b,this.d] ];b.scalex=f.sqrt(k(d[0]));p(d[0]);b.shear=d[0][0]*d[1][0]+d[0][1]*d[1][1];d[1]=[d[1][0]-d[0][0]*b.shear,d[1][1]-d[0][1]*b.shear];b.scaley=f.sqrt(k(d[1]));
p(d[1]);b.shear/=b.scaley;0>this.determinant()&&(b.scalex=-b.scalex);var e=-d[0][1],d=d[1][1];0>d?(b.rotate=a.deg(f.acos(d)),0>e&&(b.rotate=360-b.rotate)):b.rotate=a.deg(f.asin(e));b.isSimple=!+b.shear.toFixed(9)&&(b.scalex.toFixed(9)==b.scaley.toFixed(9)||!b.rotate);b.isSuperSimple=!+b.shear.toFixed(9)&&b.scalex.toFixed(9)==b.scaley.toFixed(9)&&!b.rotate;b.noRotation=!+b.shear.toFixed(9)&&!b.rotate;return b};n.toTransformString=function(a){a=a||this.split();if(+a.shear.toFixed(9))return"m"+[this.get(0),
this.get(1),this.get(2),this.get(3),this.get(4),this.get(5)];a.scalex=+a.scalex.toFixed(4);a.scaley=+a.scaley.toFixed(4);a.rotate=+a.rotate.toFixed(4);return(a.dx||a.dy?"t"+[+a.dx.toFixed(4),+a.dy.toFixed(4)]:"")+(1!=a.scalex||1!=a.scaley?"s"+[a.scalex,a.scaley,0,0]:"")+(a.rotate?"r"+[+a.rotate.toFixed(4),0,0]:"")}})(w.prototype);a.Matrix=w;a.matrix=function(a,d,f,b,k,e){return new w(a,d,f,b,k,e)}});C.plugin(function(a,v,y,M,A){function w(h){return function(d){k.stop();d instanceof A&&1==d.node.childNodes.length&&
("radialGradient"==d.node.firstChild.tagName||"linearGradient"==d.node.firstChild.tagName||"pattern"==d.node.firstChild.tagName)&&(d=d.node.firstChild,b(this).appendChild(d),d=u(d));if(d instanceof v)if("radialGradient"==d.type||"linearGradient"==d.type||"pattern"==d.type){d.node.id||e(d.node,{id:d.id});var f=l(d.node.id)}else f=d.attr(h);else f=a.color(d),f.error?(f=a(b(this).ownerSVGElement).gradient(d))?(f.node.id||e(f.node,{id:f.id}),f=l(f.node.id)):f=d:f=r(f);d={};d[h]=f;e(this.node,d);this.node.style[h]=
x}}function z(a){k.stop();a==+a&&(a+="px");this.node.style.fontSize=a}function d(a){var b=[];a=a.childNodes;for(var e=0,f=a.length;e<f;e++){var l=a[e];3==l.nodeType&&b.push(l.nodeValue);"tspan"==l.tagName&&(1==l.childNodes.length&&3==l.firstChild.nodeType?b.push(l.firstChild.nodeValue):b.push(d(l)))}return b}function f(){k.stop();return this.node.style.fontSize}var n=a._.make,u=a._.wrap,p=a.is,b=a._.getSomeDefs,q=/^url\(#?([^)]+)\)$/,e=a._.$,l=a.url,r=String,s=a._.separator,x="";k.on("snap.util.attr.mask",
function(a){if(a instanceof v||a instanceof A){k.stop();a instanceof A&&1==a.node.childNodes.length&&(a=a.node.firstChild,b(this).appendChild(a),a=u(a));if("mask"==a.type)var d=a;else d=n("mask",b(this)),d.node.appendChild(a.node);!d.node.id&&e(d.node,{id:d.id});e(this.node,{mask:l(d.id)})}});(function(a){k.on("snap.util.attr.clip",a);k.on("snap.util.attr.clip-path",a);k.on("snap.util.attr.clipPath",a)})(function(a){if(a instanceof v||a instanceof A){k.stop();if("clipPath"==a.type)var d=a;else d=
n("clipPath",b(this)),d.node.appendChild(a.node),!d.node.id&&e(d.node,{id:d.id});e(this.node,{"clip-path":l(d.id)})}});k.on("snap.util.attr.fill",w("fill"));k.on("snap.util.attr.stroke",w("stroke"));var G=/^([lr])(?:\(([^)]*)\))?(.*)$/i;k.on("snap.util.grad.parse",function(a){a=r(a);var b=a.match(G);if(!b)return null;a=b[1];var e=b[2],b=b[3],e=e.split(/\s*,\s*/).map(function(a){return+a==a?+a:a});1==e.length&&0==e[0]&&(e=[]);b=b.split("-");b=b.map(function(a){a=a.split(":");var b={color:a[0]};a[1]&&
(b.offset=parseFloat(a[1]));return b});return{type:a,params:e,stops:b}});k.on("snap.util.attr.d",function(b){k.stop();p(b,"array")&&p(b[0],"array")&&(b=a.path.toString.call(b));b=r(b);b.match(/[ruo]/i)&&(b=a.path.toAbsolute(b));e(this.node,{d:b})})(-1);k.on("snap.util.attr.#text",function(a){k.stop();a=r(a);for(a=M.doc.createTextNode(a);this.node.firstChild;)this.node.removeChild(this.node.firstChild);this.node.appendChild(a)})(-1);k.on("snap.util.attr.path",function(a){k.stop();this.attr({d:a})})(-1);
k.on("snap.util.attr.class",function(a){k.stop();this.node.className.baseVal=a})(-1);k.on("snap.util.attr.viewBox",function(a){a=p(a,"object")&&"x"in a?[a.x,a.y,a.width,a.height].join(" "):p(a,"array")?a.join(" "):a;e(this.node,{viewBox:a});k.stop()})(-1);k.on("snap.util.attr.transform",function(a){this.transform(a);k.stop()})(-1);k.on("snap.util.attr.r",function(a){"rect"==this.type&&(k.stop(),e(this.node,{rx:a,ry:a}))})(-1);k.on("snap.util.attr.textpath",function(a){k.stop();if("text"==this.type){var d,
f;if(!a&&this.textPath){for(a=this.textPath;a.node.firstChild;)this.node.appendChild(a.node.firstChild);a.remove();delete this.textPath}else if(p(a,"string")?(d=b(this),a=u(d.parentNode).path(a),d.appendChild(a.node),d=a.id,a.attr({id:d})):(a=u(a),a instanceof v&&(d=a.attr("id"),d||(d=a.id,a.attr({id:d})))),d)if(a=this.textPath,f=this.node,a)a.attr({"xlink:href":"#"+d});else{for(a=e("textPath",{"xlink:href":"#"+d});f.firstChild;)a.appendChild(f.firstChild);f.appendChild(a);this.textPath=u(a)}}})(-1);
k.on("snap.util.attr.text",function(a){if("text"==this.type){for(var b=this.node,d=function(a){var b=e("tspan");if(p(a,"array"))for(var f=0;f<a.length;f++)b.appendChild(d(a[f]));else b.appendChild(M.doc.createTextNode(a));b.normalize&&b.normalize();return b};b.firstChild;)b.removeChild(b.firstChild);for(a=d(a);a.firstChild;)b.appendChild(a.firstChild)}k.stop()})(-1);k.on("snap.util.attr.fontSize",z)(-1);k.on("snap.util.attr.font-size",z)(-1);k.on("snap.util.getattr.transform",function(){k.stop();
return this.transform()})(-1);k.on("snap.util.getattr.textpath",function(){k.stop();return this.textPath})(-1);(function(){function b(d){return function(){k.stop();var b=M.doc.defaultView.getComputedStyle(this.node,null).getPropertyValue("marker-"+d);return"none"==b?b:a(M.doc.getElementById(b.match(q)[1]))}}function d(a){return function(b){k.stop();var d="marker"+a.charAt(0).toUpperCase()+a.substring(1);if(""==b||!b)this.node.style[d]="none";else if("marker"==b.type){var f=b.node.id;f||e(b.node,{id:b.id});
this.node.style[d]=l(f)}}}k.on("snap.util.getattr.marker-end",b("end"))(-1);k.on("snap.util.getattr.markerEnd",b("end"))(-1);k.on("snap.util.getattr.marker-start",b("start"))(-1);k.on("snap.util.getattr.markerStart",b("start"))(-1);k.on("snap.util.getattr.marker-mid",b("mid"))(-1);k.on("snap.util.getattr.markerMid",b("mid"))(-1);k.on("snap.util.attr.marker-end",d("end"))(-1);k.on("snap.util.attr.markerEnd",d("end"))(-1);k.on("snap.util.attr.marker-start",d("start"))(-1);k.on("snap.util.attr.markerStart",
d("start"))(-1);k.on("snap.util.attr.marker-mid",d("mid"))(-1);k.on("snap.util.attr.markerMid",d("mid"))(-1)})();k.on("snap.util.getattr.r",function(){if("rect"==this.type&&e(this.node,"rx")==e(this.node,"ry"))return k.stop(),e(this.node,"rx")})(-1);k.on("snap.util.getattr.text",function(){if("text"==this.type||"tspan"==this.type){k.stop();var a=d(this.node);return 1==a.length?a[0]:a}})(-1);k.on("snap.util.getattr.#text",function(){return this.node.textContent})(-1);k.on("snap.util.getattr.viewBox",
function(){k.stop();var b=e(this.node,"viewBox");if(b)return b=b.split(s),a._.box(+b[0],+b[1],+b[2],+b[3])})(-1);k.on("snap.util.getattr.points",function(){var a=e(this.node,"points");k.stop();if(a)return a.split(s)})(-1);k.on("snap.util.getattr.path",function(){var a=e(this.node,"d");k.stop();return a})(-1);k.on("snap.util.getattr.class",function(){return this.node.className.baseVal})(-1);k.on("snap.util.getattr.fontSize",f)(-1);k.on("snap.util.getattr.font-size",f)(-1)});C.plugin(function(a,v,y,
M,A){function w(a){return a}function z(a){return function(b){return+b.toFixed(3)+a}}var d={"+":function(a,b){return a+b},"-":function(a,b){return a-b},"/":function(a,b){return a/b},"*":function(a,b){return a*b}},f=String,n=/[a-z]+$/i,u=/^\s*([+\-\/*])\s*=\s*([\d.eE+\-]+)\s*([^\d\s]+)?\s*$/;k.on("snap.util.attr",function(a){if(a=f(a).match(u)){var b=k.nt(),b=b.substring(b.lastIndexOf(".")+1),q=this.attr(b),e={};k.stop();var l=a[3]||"",r=q.match(n),s=d[a[1] ];r&&r==l?a=s(parseFloat(q),+a[2]):(q=this.asPX(b),
a=s(this.asPX(b),this.asPX(b,a[2]+l)));isNaN(q)||isNaN(a)||(e[b]=a,this.attr(e))}})(-10);k.on("snap.util.equal",function(a,b){var q=f(this.attr(a)||""),e=f(b).match(u);if(e){k.stop();var l=e[3]||"",r=q.match(n),s=d[e[1] ];if(r&&r==l)return{from:parseFloat(q),to:s(parseFloat(q),+e[2]),f:z(r)};q=this.asPX(a);return{from:q,to:s(q,this.asPX(a,e[2]+l)),f:w}}})(-10)});C.plugin(function(a,v,y,M,A){var w=y.prototype,z=a.is;w.rect=function(a,d,k,p,b,q){var e;null==q&&(q=b);z(a,"object")&&"[object Object]"==
a?e=a:null!=a&&(e={x:a,y:d,width:k,height:p},null!=b&&(e.rx=b,e.ry=q));return this.el("rect",e)};w.circle=function(a,d,k){var p;z(a,"object")&&"[object Object]"==a?p=a:null!=a&&(p={cx:a,cy:d,r:k});return this.el("circle",p)};var d=function(){function a(){this.parentNode.removeChild(this)}return function(d,k){var p=M.doc.createElement("img"),b=M.doc.body;p.style.cssText="position:absolute;left:-9999em;top:-9999em";p.onload=function(){k.call(p);p.onload=p.onerror=null;b.removeChild(p)};p.onerror=a;
b.appendChild(p);p.src=d}}();w.image=function(f,n,k,p,b){var q=this.el("image");if(z(f,"object")&&"src"in f)q.attr(f);else if(null!=f){var e={"xlink:href":f,preserveAspectRatio:"none"};null!=n&&null!=k&&(e.x=n,e.y=k);null!=p&&null!=b?(e.width=p,e.height=b):d(f,function(){a._.$(q.node,{width:this.offsetWidth,height:this.offsetHeight})});a._.$(q.node,e)}return q};w.ellipse=function(a,d,k,p){var b;z(a,"object")&&"[object Object]"==a?b=a:null!=a&&(b={cx:a,cy:d,rx:k,ry:p});return this.el("ellipse",b)};
w.path=function(a){var d;z(a,"object")&&!z(a,"array")?d=a:a&&(d={d:a});return this.el("path",d)};w.group=w.g=function(a){var d=this.el("g");1==arguments.length&&a&&!a.type?d.attr(a):arguments.length&&d.add(Array.prototype.slice.call(arguments,0));return d};w.svg=function(a,d,k,p,b,q,e,l){var r={};z(a,"object")&&null==d?r=a:(null!=a&&(r.x=a),null!=d&&(r.y=d),null!=k&&(r.width=k),null!=p&&(r.height=p),null!=b&&null!=q&&null!=e&&null!=l&&(r.viewBox=[b,q,e,l]));return this.el("svg",r)};w.mask=function(a){var d=
this.el("mask");1==arguments.length&&a&&!a.type?d.attr(a):arguments.length&&d.add(Array.prototype.slice.call(arguments,0));return d};w.ptrn=function(a,d,k,p,b,q,e,l){if(z(a,"object"))var r=a;else arguments.length?(r={},null!=a&&(r.x=a),null!=d&&(r.y=d),null!=k&&(r.width=k),null!=p&&(r.height=p),null!=b&&null!=q&&null!=e&&null!=l&&(r.viewBox=[b,q,e,l])):r={patternUnits:"userSpaceOnUse"};return this.el("pattern",r)};w.use=function(a){return null!=a?(make("use",this.node),a instanceof v&&(a.attr("id")||
a.attr({id:ID()}),a=a.attr("id")),this.el("use",{"xlink:href":a})):v.prototype.use.call(this)};w.text=function(a,d,k){var p={};z(a,"object")?p=a:null!=a&&(p={x:a,y:d,text:k||""});return this.el("text",p)};w.line=function(a,d,k,p){var b={};z(a,"object")?b=a:null!=a&&(b={x1:a,x2:k,y1:d,y2:p});return this.el("line",b)};w.polyline=function(a){1<arguments.length&&(a=Array.prototype.slice.call(arguments,0));var d={};z(a,"object")&&!z(a,"array")?d=a:null!=a&&(d={points:a});return this.el("polyline",d)};
w.polygon=function(a){1<arguments.length&&(a=Array.prototype.slice.call(arguments,0));var d={};z(a,"object")&&!z(a,"array")?d=a:null!=a&&(d={points:a});return this.el("polygon",d)};(function(){function d(){return this.selectAll("stop")}function n(b,d){var f=e("stop"),k={offset:+d+"%"};b=a.color(b);k["stop-color"]=b.hex;1>b.opacity&&(k["stop-opacity"]=b.opacity);e(f,k);this.node.appendChild(f);return this}function u(){if("linearGradient"==this.type){var b=e(this.node,"x1")||0,d=e(this.node,"x2")||
1,f=e(this.node,"y1")||0,k=e(this.node,"y2")||0;return a._.box(b,f,math.abs(d-b),math.abs(k-f))}b=this.node.r||0;return a._.box((this.node.cx||0.5)-b,(this.node.cy||0.5)-b,2*b,2*b)}function p(a,d){function f(a,b){for(var d=(b-u)/(a-w),e=w;e<a;e++)h[e].offset=+(+u+d*(e-w)).toFixed(2);w=a;u=b}var n=k("snap.util.grad.parse",null,d).firstDefined(),p;if(!n)return null;n.params.unshift(a);p="l"==n.type.toLowerCase()?b.apply(0,n.params):q.apply(0,n.params);n.type!=n.type.toLowerCase()&&e(p.node,{gradientUnits:"userSpaceOnUse"});
var h=n.stops,n=h.length,u=0,w=0;n--;for(var v=0;v<n;v++)"offset"in h[v]&&f(v,h[v].offset);h[n].offset=h[n].offset||100;f(n,h[n].offset);for(v=0;v<=n;v++){var y=h[v];p.addStop(y.color,y.offset)}return p}function b(b,k,p,q,w){b=a._.make("linearGradient",b);b.stops=d;b.addStop=n;b.getBBox=u;null!=k&&e(b.node,{x1:k,y1:p,x2:q,y2:w});return b}function q(b,k,p,q,w,h){b=a._.make("radialGradient",b);b.stops=d;b.addStop=n;b.getBBox=u;null!=k&&e(b.node,{cx:k,cy:p,r:q});null!=w&&null!=h&&e(b.node,{fx:w,fy:h});
return b}var e=a._.$;w.gradient=function(a){return p(this.defs,a)};w.gradientLinear=function(a,d,e,f){return b(this.defs,a,d,e,f)};w.gradientRadial=function(a,b,d,e,f){return q(this.defs,a,b,d,e,f)};w.toString=function(){var b=this.node.ownerDocument,d=b.createDocumentFragment(),b=b.createElement("div"),e=this.node.cloneNode(!0);d.appendChild(b);b.appendChild(e);a._.$(e,{xmlns:"http://www.w3.org/2000/svg"});b=b.innerHTML;d.removeChild(d.firstChild);return b};w.clear=function(){for(var a=this.node.firstChild,
b;a;)b=a.nextSibling,"defs"!=a.tagName?a.parentNode.removeChild(a):w.clear.call({node:a}),a=b}})()});C.plugin(function(a,k,y,M){function A(a){var b=A.ps=A.ps||{};b[a]?b[a].sleep=100:b[a]={sleep:100};setTimeout(function(){for(var d in b)b[L](d)&&d!=a&&(b[d].sleep--,!b[d].sleep&&delete b[d])});return b[a]}function w(a,b,d,e){null==a&&(a=b=d=e=0);null==b&&(b=a.y,d=a.width,e=a.height,a=a.x);return{x:a,y:b,width:d,w:d,height:e,h:e,x2:a+d,y2:b+e,cx:a+d/2,cy:b+e/2,r1:F.min(d,e)/2,r2:F.max(d,e)/2,r0:F.sqrt(d*
d+e*e)/2,path:s(a,b,d,e),vb:[a,b,d,e].join(" ")}}function z(){return this.join(",").replace(N,"$1")}function d(a){a=C(a);a.toString=z;return a}function f(a,b,d,h,f,k,l,n,p){if(null==p)return e(a,b,d,h,f,k,l,n);if(0>p||e(a,b,d,h,f,k,l,n)<p)p=void 0;else{var q=0.5,O=1-q,s;for(s=e(a,b,d,h,f,k,l,n,O);0.01<Z(s-p);)q/=2,O+=(s<p?1:-1)*q,s=e(a,b,d,h,f,k,l,n,O);p=O}return u(a,b,d,h,f,k,l,n,p)}function n(b,d){function e(a){return+(+a).toFixed(3)}return a._.cacher(function(a,h,l){a instanceof k&&(a=a.attr("d"));
a=I(a);for(var n,p,D,q,O="",s={},c=0,t=0,r=a.length;t<r;t++){D=a[t];if("M"==D[0])n=+D[1],p=+D[2];else{q=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6]);if(c+q>h){if(d&&!s.start){n=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6],h-c);O+=["C"+e(n.start.x),e(n.start.y),e(n.m.x),e(n.m.y),e(n.x),e(n.y)];if(l)return O;s.start=O;O=["M"+e(n.x),e(n.y)+"C"+e(n.n.x),e(n.n.y),e(n.end.x),e(n.end.y),e(D[5]),e(D[6])].join();c+=q;n=+D[5];p=+D[6];continue}if(!b&&!d)return n=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6],h-c)}c+=q;n=+D[5];p=+D[6]}O+=
D.shift()+D}s.end=O;return n=b?c:d?s:u(n,p,D[0],D[1],D[2],D[3],D[4],D[5],1)},null,a._.clone)}function u(a,b,d,e,h,f,k,l,n){var p=1-n,q=ma(p,3),s=ma(p,2),c=n*n,t=c*n,r=q*a+3*s*n*d+3*p*n*n*h+t*k,q=q*b+3*s*n*e+3*p*n*n*f+t*l,s=a+2*n*(d-a)+c*(h-2*d+a),t=b+2*n*(e-b)+c*(f-2*e+b),x=d+2*n*(h-d)+c*(k-2*h+d),c=e+2*n*(f-e)+c*(l-2*f+e);a=p*a+n*d;b=p*b+n*e;h=p*h+n*k;f=p*f+n*l;l=90-180*F.atan2(s-x,t-c)/S;return{x:r,y:q,m:{x:s,y:t},n:{x:x,y:c},start:{x:a,y:b},end:{x:h,y:f},alpha:l}}function p(b,d,e,h,f,n,k,l){a.is(b,
"array")||(b=[b,d,e,h,f,n,k,l]);b=U.apply(null,b);return w(b.min.x,b.min.y,b.max.x-b.min.x,b.max.y-b.min.y)}function b(a,b,d){return b>=a.x&&b<=a.x+a.width&&d>=a.y&&d<=a.y+a.height}function q(a,d){a=w(a);d=w(d);return b(d,a.x,a.y)||b(d,a.x2,a.y)||b(d,a.x,a.y2)||b(d,a.x2,a.y2)||b(a,d.x,d.y)||b(a,d.x2,d.y)||b(a,d.x,d.y2)||b(a,d.x2,d.y2)||(a.x<d.x2&&a.x>d.x||d.x<a.x2&&d.x>a.x)&&(a.y<d.y2&&a.y>d.y||d.y<a.y2&&d.y>a.y)}function e(a,b,d,e,h,f,n,k,l){null==l&&(l=1);l=(1<l?1:0>l?0:l)/2;for(var p=[-0.1252,
0.1252,-0.3678,0.3678,-0.5873,0.5873,-0.7699,0.7699,-0.9041,0.9041,-0.9816,0.9816],q=[0.2491,0.2491,0.2335,0.2335,0.2032,0.2032,0.1601,0.1601,0.1069,0.1069,0.0472,0.0472],s=0,c=0;12>c;c++)var t=l*p[c]+l,r=t*(t*(-3*a+9*d-9*h+3*n)+6*a-12*d+6*h)-3*a+3*d,t=t*(t*(-3*b+9*e-9*f+3*k)+6*b-12*e+6*f)-3*b+3*e,s=s+q[c]*F.sqrt(r*r+t*t);return l*s}function l(a,b,d){a=I(a);b=I(b);for(var h,f,l,n,k,s,r,O,x,c,t=d?0:[],w=0,v=a.length;w<v;w++)if(x=a[w],"M"==x[0])h=k=x[1],f=s=x[2];else{"C"==x[0]?(x=[h,f].concat(x.slice(1)),
h=x[6],f=x[7]):(x=[h,f,h,f,k,s,k,s],h=k,f=s);for(var G=0,y=b.length;G<y;G++)if(c=b[G],"M"==c[0])l=r=c[1],n=O=c[2];else{"C"==c[0]?(c=[l,n].concat(c.slice(1)),l=c[6],n=c[7]):(c=[l,n,l,n,r,O,r,O],l=r,n=O);var z;var K=x,B=c;z=d;var H=p(K),J=p(B);if(q(H,J)){for(var H=e.apply(0,K),J=e.apply(0,B),H=~~(H/8),J=~~(J/8),U=[],A=[],F={},M=z?0:[],P=0;P<H+1;P++){var C=u.apply(0,K.concat(P/H));U.push({x:C.x,y:C.y,t:P/H})}for(P=0;P<J+1;P++)C=u.apply(0,B.concat(P/J)),A.push({x:C.x,y:C.y,t:P/J});for(P=0;P<H;P++)for(K=
0;K<J;K++){var Q=U[P],L=U[P+1],B=A[K],C=A[K+1],N=0.001>Z(L.x-Q.x)?"y":"x",S=0.001>Z(C.x-B.x)?"y":"x",R;R=Q.x;var Y=Q.y,V=L.x,ea=L.y,fa=B.x,ga=B.y,ha=C.x,ia=C.y;if(W(R,V)<X(fa,ha)||X(R,V)>W(fa,ha)||W(Y,ea)<X(ga,ia)||X(Y,ea)>W(ga,ia))R=void 0;else{var $=(R*ea-Y*V)*(fa-ha)-(R-V)*(fa*ia-ga*ha),aa=(R*ea-Y*V)*(ga-ia)-(Y-ea)*(fa*ia-ga*ha),ja=(R-V)*(ga-ia)-(Y-ea)*(fa-ha);if(ja){var $=$/ja,aa=aa/ja,ja=+$.toFixed(2),ba=+aa.toFixed(2);R=ja<+X(R,V).toFixed(2)||ja>+W(R,V).toFixed(2)||ja<+X(fa,ha).toFixed(2)||
ja>+W(fa,ha).toFixed(2)||ba<+X(Y,ea).toFixed(2)||ba>+W(Y,ea).toFixed(2)||ba<+X(ga,ia).toFixed(2)||ba>+W(ga,ia).toFixed(2)?void 0:{x:$,y:aa}}else R=void 0}R&&F[R.x.toFixed(4)]!=R.y.toFixed(4)&&(F[R.x.toFixed(4)]=R.y.toFixed(4),Q=Q.t+Z((R[N]-Q[N])/(L[N]-Q[N]))*(L.t-Q.t),B=B.t+Z((R[S]-B[S])/(C[S]-B[S]))*(C.t-B.t),0<=Q&&1>=Q&&0<=B&&1>=B&&(z?M++:M.push({x:R.x,y:R.y,t1:Q,t2:B})))}z=M}else z=z?0:[];if(d)t+=z;else{H=0;for(J=z.length;H<J;H++)z[H].segment1=w,z[H].segment2=G,z[H].bez1=x,z[H].bez2=c;t=t.concat(z)}}}return t}
function r(a){var b=A(a);if(b.bbox)return C(b.bbox);if(!a)return w();a=I(a);for(var d=0,e=0,h=[],f=[],l,n=0,k=a.length;n<k;n++)l=a[n],"M"==l[0]?(d=l[1],e=l[2],h.push(d),f.push(e)):(d=U(d,e,l[1],l[2],l[3],l[4],l[5],l[6]),h=h.concat(d.min.x,d.max.x),f=f.concat(d.min.y,d.max.y),d=l[5],e=l[6]);a=X.apply(0,h);l=X.apply(0,f);h=W.apply(0,h);f=W.apply(0,f);f=w(a,l,h-a,f-l);b.bbox=C(f);return f}function s(a,b,d,e,h){if(h)return[["M",+a+ +h,b],["l",d-2*h,0],["a",h,h,0,0,1,h,h],["l",0,e-2*h],["a",h,h,0,0,1,
-h,h],["l",2*h-d,0],["a",h,h,0,0,1,-h,-h],["l",0,2*h-e],["a",h,h,0,0,1,h,-h],["z"] ];a=[["M",a,b],["l",d,0],["l",0,e],["l",-d,0],["z"] ];a.toString=z;return a}function x(a,b,d,e,h){null==h&&null==e&&(e=d);a=+a;b=+b;d=+d;e=+e;if(null!=h){var f=Math.PI/180,l=a+d*Math.cos(-e*f);a+=d*Math.cos(-h*f);var n=b+d*Math.sin(-e*f);b+=d*Math.sin(-h*f);d=[["M",l,n],["A",d,d,0,+(180<h-e),0,a,b] ]}else d=[["M",a,b],["m",0,-e],["a",d,e,0,1,1,0,2*e],["a",d,e,0,1,1,0,-2*e],["z"] ];d.toString=z;return d}function G(b){var e=
A(b);if(e.abs)return d(e.abs);Q(b,"array")&&Q(b&&b[0],"array")||(b=a.parsePathString(b));if(!b||!b.length)return[["M",0,0] ];var h=[],f=0,l=0,n=0,k=0,p=0;"M"==b[0][0]&&(f=+b[0][1],l=+b[0][2],n=f,k=l,p++,h[0]=["M",f,l]);for(var q=3==b.length&&"M"==b[0][0]&&"R"==b[1][0].toUpperCase()&&"Z"==b[2][0].toUpperCase(),s,r,w=p,c=b.length;w<c;w++){h.push(s=[]);r=b[w];p=r[0];if(p!=p.toUpperCase())switch(s[0]=p.toUpperCase(),s[0]){case "A":s[1]=r[1];s[2]=r[2];s[3]=r[3];s[4]=r[4];s[5]=r[5];s[6]=+r[6]+f;s[7]=+r[7]+
l;break;case "V":s[1]=+r[1]+l;break;case "H":s[1]=+r[1]+f;break;case "R":for(var t=[f,l].concat(r.slice(1)),u=2,v=t.length;u<v;u++)t[u]=+t[u]+f,t[++u]=+t[u]+l;h.pop();h=h.concat(P(t,q));break;case "O":h.pop();t=x(f,l,r[1],r[2]);t.push(t[0]);h=h.concat(t);break;case "U":h.pop();h=h.concat(x(f,l,r[1],r[2],r[3]));s=["U"].concat(h[h.length-1].slice(-2));break;case "M":n=+r[1]+f,k=+r[2]+l;default:for(u=1,v=r.length;u<v;u++)s[u]=+r[u]+(u%2?f:l)}else if("R"==p)t=[f,l].concat(r.slice(1)),h.pop(),h=h.concat(P(t,
q)),s=["R"].concat(r.slice(-2));else if("O"==p)h.pop(),t=x(f,l,r[1],r[2]),t.push(t[0]),h=h.concat(t);else if("U"==p)h.pop(),h=h.concat(x(f,l,r[1],r[2],r[3])),s=["U"].concat(h[h.length-1].slice(-2));else for(t=0,u=r.length;t<u;t++)s[t]=r[t];p=p.toUpperCase();if("O"!=p)switch(s[0]){case "Z":f=+n;l=+k;break;case "H":f=s[1];break;case "V":l=s[1];break;case "M":n=s[s.length-2],k=s[s.length-1];default:f=s[s.length-2],l=s[s.length-1]}}h.toString=z;e.abs=d(h);return h}function h(a,b,d,e){return[a,b,d,e,d,
e]}function J(a,b,d,e,h,f){var l=1/3,n=2/3;return[l*a+n*d,l*b+n*e,l*h+n*d,l*f+n*e,h,f]}function K(b,d,e,h,f,l,n,k,p,s){var r=120*S/180,q=S/180*(+f||0),c=[],t,x=a._.cacher(function(a,b,c){var d=a*F.cos(c)-b*F.sin(c);a=a*F.sin(c)+b*F.cos(c);return{x:d,y:a}});if(s)v=s[0],t=s[1],l=s[2],u=s[3];else{t=x(b,d,-q);b=t.x;d=t.y;t=x(k,p,-q);k=t.x;p=t.y;F.cos(S/180*f);F.sin(S/180*f);t=(b-k)/2;v=(d-p)/2;u=t*t/(e*e)+v*v/(h*h);1<u&&(u=F.sqrt(u),e*=u,h*=u);var u=e*e,w=h*h,u=(l==n?-1:1)*F.sqrt(Z((u*w-u*v*v-w*t*t)/
(u*v*v+w*t*t)));l=u*e*v/h+(b+k)/2;var u=u*-h*t/e+(d+p)/2,v=F.asin(((d-u)/h).toFixed(9));t=F.asin(((p-u)/h).toFixed(9));v=b<l?S-v:v;t=k<l?S-t:t;0>v&&(v=2*S+v);0>t&&(t=2*S+t);n&&v>t&&(v-=2*S);!n&&t>v&&(t-=2*S)}if(Z(t-v)>r){var c=t,w=k,G=p;t=v+r*(n&&t>v?1:-1);k=l+e*F.cos(t);p=u+h*F.sin(t);c=K(k,p,e,h,f,0,n,w,G,[t,c,l,u])}l=t-v;f=F.cos(v);r=F.sin(v);n=F.cos(t);t=F.sin(t);l=F.tan(l/4);e=4/3*e*l;l*=4/3*h;h=[b,d];b=[b+e*r,d-l*f];d=[k+e*t,p-l*n];k=[k,p];b[0]=2*h[0]-b[0];b[1]=2*h[1]-b[1];if(s)return[b,d,k].concat(c);
c=[b,d,k].concat(c).join().split(",");s=[];k=0;for(p=c.length;k<p;k++)s[k]=k%2?x(c[k-1],c[k],q).y:x(c[k],c[k+1],q).x;return s}function U(a,b,d,e,h,f,l,k){for(var n=[],p=[[],[] ],s,r,c,t,q=0;2>q;++q)0==q?(r=6*a-12*d+6*h,s=-3*a+9*d-9*h+3*l,c=3*d-3*a):(r=6*b-12*e+6*f,s=-3*b+9*e-9*f+3*k,c=3*e-3*b),1E-12>Z(s)?1E-12>Z(r)||(s=-c/r,0<s&&1>s&&n.push(s)):(t=r*r-4*c*s,c=F.sqrt(t),0>t||(t=(-r+c)/(2*s),0<t&&1>t&&n.push(t),s=(-r-c)/(2*s),0<s&&1>s&&n.push(s)));for(r=q=n.length;q--;)s=n[q],c=1-s,p[0][q]=c*c*c*a+3*
c*c*s*d+3*c*s*s*h+s*s*s*l,p[1][q]=c*c*c*b+3*c*c*s*e+3*c*s*s*f+s*s*s*k;p[0][r]=a;p[1][r]=b;p[0][r+1]=l;p[1][r+1]=k;p[0].length=p[1].length=r+2;return{min:{x:X.apply(0,p[0]),y:X.apply(0,p[1])},max:{x:W.apply(0,p[0]),y:W.apply(0,p[1])}}}function I(a,b){var e=!b&&A(a);if(!b&&e.curve)return d(e.curve);var f=G(a),l=b&&G(b),n={x:0,y:0,bx:0,by:0,X:0,Y:0,qx:null,qy:null},k={x:0,y:0,bx:0,by:0,X:0,Y:0,qx:null,qy:null},p=function(a,b,c){if(!a)return["C",b.x,b.y,b.x,b.y,b.x,b.y];a[0]in{T:1,Q:1}||(b.qx=b.qy=null);
switch(a[0]){case "M":b.X=a[1];b.Y=a[2];break;case "A":a=["C"].concat(K.apply(0,[b.x,b.y].concat(a.slice(1))));break;case "S":"C"==c||"S"==c?(c=2*b.x-b.bx,b=2*b.y-b.by):(c=b.x,b=b.y);a=["C",c,b].concat(a.slice(1));break;case "T":"Q"==c||"T"==c?(b.qx=2*b.x-b.qx,b.qy=2*b.y-b.qy):(b.qx=b.x,b.qy=b.y);a=["C"].concat(J(b.x,b.y,b.qx,b.qy,a[1],a[2]));break;case "Q":b.qx=a[1];b.qy=a[2];a=["C"].concat(J(b.x,b.y,a[1],a[2],a[3],a[4]));break;case "L":a=["C"].concat(h(b.x,b.y,a[1],a[2]));break;case "H":a=["C"].concat(h(b.x,
b.y,a[1],b.y));break;case "V":a=["C"].concat(h(b.x,b.y,b.x,a[1]));break;case "Z":a=["C"].concat(h(b.x,b.y,b.X,b.Y))}return a},s=function(a,b){if(7<a[b].length){a[b].shift();for(var c=a[b];c.length;)q[b]="A",l&&(u[b]="A"),a.splice(b++,0,["C"].concat(c.splice(0,6)));a.splice(b,1);v=W(f.length,l&&l.length||0)}},r=function(a,b,c,d,e){a&&b&&"M"==a[e][0]&&"M"!=b[e][0]&&(b.splice(e,0,["M",d.x,d.y]),c.bx=0,c.by=0,c.x=a[e][1],c.y=a[e][2],v=W(f.length,l&&l.length||0))},q=[],u=[],c="",t="",x=0,v=W(f.length,
l&&l.length||0);for(;x<v;x++){f[x]&&(c=f[x][0]);"C"!=c&&(q[x]=c,x&&(t=q[x-1]));f[x]=p(f[x],n,t);"A"!=q[x]&&"C"==c&&(q[x]="C");s(f,x);l&&(l[x]&&(c=l[x][0]),"C"!=c&&(u[x]=c,x&&(t=u[x-1])),l[x]=p(l[x],k,t),"A"!=u[x]&&"C"==c&&(u[x]="C"),s(l,x));r(f,l,n,k,x);r(l,f,k,n,x);var w=f[x],z=l&&l[x],y=w.length,U=l&&z.length;n.x=w[y-2];n.y=w[y-1];n.bx=$(w[y-4])||n.x;n.by=$(w[y-3])||n.y;k.bx=l&&($(z[U-4])||k.x);k.by=l&&($(z[U-3])||k.y);k.x=l&&z[U-2];k.y=l&&z[U-1]}l||(e.curve=d(f));return l?[f,l]:f}function P(a,
b){for(var d=[],e=0,h=a.length;h-2*!b>e;e+=2){var f=[{x:+a[e-2],y:+a[e-1]},{x:+a[e],y:+a[e+1]},{x:+a[e+2],y:+a[e+3]},{x:+a[e+4],y:+a[e+5]}];b?e?h-4==e?f[3]={x:+a[0],y:+a[1]}:h-2==e&&(f[2]={x:+a[0],y:+a[1]},f[3]={x:+a[2],y:+a[3]}):f[0]={x:+a[h-2],y:+a[h-1]}:h-4==e?f[3]=f[2]:e||(f[0]={x:+a[e],y:+a[e+1]});d.push(["C",(-f[0].x+6*f[1].x+f[2].x)/6,(-f[0].y+6*f[1].y+f[2].y)/6,(f[1].x+6*f[2].x-f[3].x)/6,(f[1].y+6*f[2].y-f[3].y)/6,f[2].x,f[2].y])}return d}y=k.prototype;var Q=a.is,C=a._.clone,L="hasOwnProperty",
N=/,?([a-z]),?/gi,$=parseFloat,F=Math,S=F.PI,X=F.min,W=F.max,ma=F.pow,Z=F.abs;M=n(1);var na=n(),ba=n(0,1),V=a._unit2px;a.path=A;a.path.getTotalLength=M;a.path.getPointAtLength=na;a.path.getSubpath=function(a,b,d){if(1E-6>this.getTotalLength(a)-d)return ba(a,b).end;a=ba(a,d,1);return b?ba(a,b).end:a};y.getTotalLength=function(){if(this.node.getTotalLength)return this.node.getTotalLength()};y.getPointAtLength=function(a){return na(this.attr("d"),a)};y.getSubpath=function(b,d){return a.path.getSubpath(this.attr("d"),
b,d)};a._.box=w;a.path.findDotsAtSegment=u;a.path.bezierBBox=p;a.path.isPointInsideBBox=b;a.path.isBBoxIntersect=q;a.path.intersection=function(a,b){return l(a,b)};a.path.intersectionNumber=function(a,b){return l(a,b,1)};a.path.isPointInside=function(a,d,e){var h=r(a);return b(h,d,e)&&1==l(a,[["M",d,e],["H",h.x2+10] ],1)%2};a.path.getBBox=r;a.path.get={path:function(a){return a.attr("path")},circle:function(a){a=V(a);return x(a.cx,a.cy,a.r)},ellipse:function(a){a=V(a);return x(a.cx||0,a.cy||0,a.rx,
a.ry)},rect:function(a){a=V(a);return s(a.x||0,a.y||0,a.width,a.height,a.rx,a.ry)},image:function(a){a=V(a);return s(a.x||0,a.y||0,a.width,a.height)},line:function(a){return"M"+[a.attr("x1")||0,a.attr("y1")||0,a.attr("x2"),a.attr("y2")]},polyline:function(a){return"M"+a.attr("points")},polygon:function(a){return"M"+a.attr("points")+"z"},deflt:function(a){a=a.node.getBBox();return s(a.x,a.y,a.width,a.height)}};a.path.toRelative=function(b){var e=A(b),h=String.prototype.toLowerCase;if(e.rel)return d(e.rel);
a.is(b,"array")&&a.is(b&&b[0],"array")||(b=a.parsePathString(b));var f=[],l=0,n=0,k=0,p=0,s=0;"M"==b[0][0]&&(l=b[0][1],n=b[0][2],k=l,p=n,s++,f.push(["M",l,n]));for(var r=b.length;s<r;s++){var q=f[s]=[],x=b[s];if(x[0]!=h.call(x[0]))switch(q[0]=h.call(x[0]),q[0]){case "a":q[1]=x[1];q[2]=x[2];q[3]=x[3];q[4]=x[4];q[5]=x[5];q[6]=+(x[6]-l).toFixed(3);q[7]=+(x[7]-n).toFixed(3);break;case "v":q[1]=+(x[1]-n).toFixed(3);break;case "m":k=x[1],p=x[2];default:for(var c=1,t=x.length;c<t;c++)q[c]=+(x[c]-(c%2?l:
n)).toFixed(3)}else for(f[s]=[],"m"==x[0]&&(k=x[1]+l,p=x[2]+n),q=0,c=x.length;q<c;q++)f[s][q]=x[q];x=f[s].length;switch(f[s][0]){case "z":l=k;n=p;break;case "h":l+=+f[s][x-1];break;case "v":n+=+f[s][x-1];break;default:l+=+f[s][x-2],n+=+f[s][x-1]}}f.toString=z;e.rel=d(f);return f};a.path.toAbsolute=G;a.path.toCubic=I;a.path.map=function(a,b){if(!b)return a;var d,e,h,f,l,n,k;a=I(a);h=0;for(l=a.length;h<l;h++)for(k=a[h],f=1,n=k.length;f<n;f+=2)d=b.x(k[f],k[f+1]),e=b.y(k[f],k[f+1]),k[f]=d,k[f+1]=e;return a};
a.path.toString=z;a.path.clone=d});C.plugin(function(a,v,y,C){var A=Math.max,w=Math.min,z=function(a){this.items=[];this.bindings={};this.length=0;this.type="set";if(a)for(var f=0,n=a.length;f<n;f++)a[f]&&(this[this.items.length]=this.items[this.items.length]=a[f],this.length++)};v=z.prototype;v.push=function(){for(var a,f,n=0,k=arguments.length;n<k;n++)if(a=arguments[n])f=this.items.length,this[f]=this.items[f]=a,this.length++;return this};v.pop=function(){this.length&&delete this[this.length--];
return this.items.pop()};v.forEach=function(a,f){for(var n=0,k=this.items.length;n<k&&!1!==a.call(f,this.items[n],n);n++);return this};v.animate=function(d,f,n,u){"function"!=typeof n||n.length||(u=n,n=L.linear);d instanceof a._.Animation&&(u=d.callback,n=d.easing,f=n.dur,d=d.attr);var p=arguments;if(a.is(d,"array")&&a.is(p[p.length-1],"array"))var b=!0;var q,e=function(){q?this.b=q:q=this.b},l=0,r=u&&function(){l++==this.length&&u.call(this)};return this.forEach(function(a,l){k.once("snap.animcreated."+
a.id,e);b?p[l]&&a.animate.apply(a,p[l]):a.animate(d,f,n,r)})};v.remove=function(){for(;this.length;)this.pop().remove();return this};v.bind=function(a,f,k){var u={};if("function"==typeof f)this.bindings[a]=f;else{var p=k||a;this.bindings[a]=function(a){u[p]=a;f.attr(u)}}return this};v.attr=function(a){var f={},k;for(k in a)if(this.bindings[k])this.bindings[k](a[k]);else f[k]=a[k];a=0;for(k=this.items.length;a<k;a++)this.items[a].attr(f);return this};v.clear=function(){for(;this.length;)this.pop()};
v.splice=function(a,f,k){a=0>a?A(this.length+a,0):a;f=A(0,w(this.length-a,f));var u=[],p=[],b=[],q;for(q=2;q<arguments.length;q++)b.push(arguments[q]);for(q=0;q<f;q++)p.push(this[a+q]);for(;q<this.length-a;q++)u.push(this[a+q]);var e=b.length;for(q=0;q<e+u.length;q++)this.items[a+q]=this[a+q]=q<e?b[q]:u[q-e];for(q=this.items.length=this.length-=f-e;this[q];)delete this[q++];return new z(p)};v.exclude=function(a){for(var f=0,k=this.length;f<k;f++)if(this[f]==a)return this.splice(f,1),!0;return!1};
v.insertAfter=function(a){for(var f=this.items.length;f--;)this.items[f].insertAfter(a);return this};v.getBBox=function(){for(var a=[],f=[],k=[],u=[],p=this.items.length;p--;)if(!this.items[p].removed){var b=this.items[p].getBBox();a.push(b.x);f.push(b.y);k.push(b.x+b.width);u.push(b.y+b.height)}a=w.apply(0,a);f=w.apply(0,f);k=A.apply(0,k);u=A.apply(0,u);return{x:a,y:f,x2:k,y2:u,width:k-a,height:u-f,cx:a+(k-a)/2,cy:f+(u-f)/2}};v.clone=function(a){a=new z;for(var f=0,k=this.items.length;f<k;f++)a.push(this.items[f].clone());
return a};v.toString=function(){return"Snap\u2018s set"};v.type="set";a.set=function(){var a=new z;arguments.length&&a.push.apply(a,Array.prototype.slice.call(arguments,0));return a}});C.plugin(function(a,v,y,C){function A(a){var b=a[0];switch(b.toLowerCase()){case "t":return[b,0,0];case "m":return[b,1,0,0,1,0,0];case "r":return 4==a.length?[b,0,a[2],a[3] ]:[b,0];case "s":return 5==a.length?[b,1,1,a[3],a[4] ]:3==a.length?[b,1,1]:[b,1]}}function w(b,d,f){d=q(d).replace(/\.{3}|\u2026/g,b);b=a.parseTransformString(b)||
[];d=a.parseTransformString(d)||[];for(var k=Math.max(b.length,d.length),p=[],v=[],h=0,w,z,y,I;h<k;h++){y=b[h]||A(d[h]);I=d[h]||A(y);if(y[0]!=I[0]||"r"==y[0].toLowerCase()&&(y[2]!=I[2]||y[3]!=I[3])||"s"==y[0].toLowerCase()&&(y[3]!=I[3]||y[4]!=I[4])){b=a._.transform2matrix(b,f());d=a._.transform2matrix(d,f());p=[["m",b.a,b.b,b.c,b.d,b.e,b.f] ];v=[["m",d.a,d.b,d.c,d.d,d.e,d.f] ];break}p[h]=[];v[h]=[];w=0;for(z=Math.max(y.length,I.length);w<z;w++)w in y&&(p[h][w]=y[w]),w in I&&(v[h][w]=I[w])}return{from:u(p),
to:u(v),f:n(p)}}function z(a){return a}function d(a){return function(b){return+b.toFixed(3)+a}}function f(b){return a.rgb(b[0],b[1],b[2])}function n(a){var b=0,d,f,k,n,h,p,q=[];d=0;for(f=a.length;d<f;d++){h="[";p=['"'+a[d][0]+'"'];k=1;for(n=a[d].length;k<n;k++)p[k]="val["+b++ +"]";h+=p+"]";q[d]=h}return Function("val","return Snap.path.toString.call(["+q+"])")}function u(a){for(var b=[],d=0,f=a.length;d<f;d++)for(var k=1,n=a[d].length;k<n;k++)b.push(a[d][k]);return b}var p={},b=/[a-z]+$/i,q=String;
p.stroke=p.fill="colour";v.prototype.equal=function(a,b){return k("snap.util.equal",this,a,b).firstDefined()};k.on("snap.util.equal",function(e,k){var r,s;r=q(this.attr(e)||"");var x=this;if(r==+r&&k==+k)return{from:+r,to:+k,f:z};if("colour"==p[e])return r=a.color(r),s=a.color(k),{from:[r.r,r.g,r.b,r.opacity],to:[s.r,s.g,s.b,s.opacity],f:f};if("transform"==e||"gradientTransform"==e||"patternTransform"==e)return k instanceof a.Matrix&&(k=k.toTransformString()),a._.rgTransform.test(k)||(k=a._.svgTransform2string(k)),
w(r,k,function(){return x.getBBox(1)});if("d"==e||"path"==e)return r=a.path.toCubic(r,k),{from:u(r[0]),to:u(r[1]),f:n(r[0])};if("points"==e)return r=q(r).split(a._.separator),s=q(k).split(a._.separator),{from:r,to:s,f:function(a){return a}};aUnit=r.match(b);s=q(k).match(b);return aUnit&&aUnit==s?{from:parseFloat(r),to:parseFloat(k),f:d(aUnit)}:{from:this.asPX(e),to:this.asPX(e,k),f:z}})});C.plugin(function(a,v,y,C){var A=v.prototype,w="createTouch"in C.doc;v="click dblclick mousedown mousemove mouseout mouseover mouseup touchstart touchmove touchend touchcancel".split(" ");
var z={mousedown:"touchstart",mousemove:"touchmove",mouseup:"touchend"},d=function(a,b){var d="y"==a?"scrollTop":"scrollLeft",e=b&&b.node?b.node.ownerDocument:C.doc;return e[d in e.documentElement?"documentElement":"body"][d]},f=function(){this.returnValue=!1},n=function(){return this.originalEvent.preventDefault()},u=function(){this.cancelBubble=!0},p=function(){return this.originalEvent.stopPropagation()},b=function(){if(C.doc.addEventListener)return function(a,b,e,f){var k=w&&z[b]?z[b]:b,l=function(k){var l=
d("y",f),q=d("x",f);if(w&&z.hasOwnProperty(b))for(var r=0,u=k.targetTouches&&k.targetTouches.length;r<u;r++)if(k.targetTouches[r].target==a||a.contains(k.targetTouches[r].target)){u=k;k=k.targetTouches[r];k.originalEvent=u;k.preventDefault=n;k.stopPropagation=p;break}return e.call(f,k,k.clientX+q,k.clientY+l)};b!==k&&a.addEventListener(b,l,!1);a.addEventListener(k,l,!1);return function(){b!==k&&a.removeEventListener(b,l,!1);a.removeEventListener(k,l,!1);return!0}};if(C.doc.attachEvent)return function(a,
b,e,h){var k=function(a){a=a||h.node.ownerDocument.window.event;var b=d("y",h),k=d("x",h),k=a.clientX+k,b=a.clientY+b;a.preventDefault=a.preventDefault||f;a.stopPropagation=a.stopPropagation||u;return e.call(h,a,k,b)};a.attachEvent("on"+b,k);return function(){a.detachEvent("on"+b,k);return!0}}}(),q=[],e=function(a){for(var b=a.clientX,e=a.clientY,f=d("y"),l=d("x"),n,p=q.length;p--;){n=q[p];if(w)for(var r=a.touches&&a.touches.length,u;r--;){if(u=a.touches[r],u.identifier==n.el._drag.id||n.el.node.contains(u.target)){b=
u.clientX;e=u.clientY;(a.originalEvent?a.originalEvent:a).preventDefault();break}}else a.preventDefault();b+=l;e+=f;k("snap.drag.move."+n.el.id,n.move_scope||n.el,b-n.el._drag.x,e-n.el._drag.y,b,e,a)}},l=function(b){a.unmousemove(e).unmouseup(l);for(var d=q.length,f;d--;)f=q[d],f.el._drag={},k("snap.drag.end."+f.el.id,f.end_scope||f.start_scope||f.move_scope||f.el,b);q=[]};for(y=v.length;y--;)(function(d){a[d]=A[d]=function(e,f){a.is(e,"function")&&(this.events=this.events||[],this.events.push({name:d,
f:e,unbind:b(this.node||document,d,e,f||this)}));return this};a["un"+d]=A["un"+d]=function(a){for(var b=this.events||[],e=b.length;e--;)if(b[e].name==d&&(b[e].f==a||!a)){b[e].unbind();b.splice(e,1);!b.length&&delete this.events;break}return this}})(v[y]);A.hover=function(a,b,d,e){return this.mouseover(a,d).mouseout(b,e||d)};A.unhover=function(a,b){return this.unmouseover(a).unmouseout(b)};var r=[];A.drag=function(b,d,f,h,n,p){function u(r,v,w){(r.originalEvent||r).preventDefault();this._drag.x=v;
this._drag.y=w;this._drag.id=r.identifier;!q.length&&a.mousemove(e).mouseup(l);q.push({el:this,move_scope:h,start_scope:n,end_scope:p});d&&k.on("snap.drag.start."+this.id,d);b&&k.on("snap.drag.move."+this.id,b);f&&k.on("snap.drag.end."+this.id,f);k("snap.drag.start."+this.id,n||h||this,v,w,r)}if(!arguments.length){var v;return this.drag(function(a,b){this.attr({transform:v+(v?"T":"t")+[a,b]})},function(){v=this.transform().local})}this._drag={};r.push({el:this,start:u});this.mousedown(u);return this};
A.undrag=function(){for(var b=r.length;b--;)r[b].el==this&&(this.unmousedown(r[b].start),r.splice(b,1),k.unbind("snap.drag.*."+this.id));!r.length&&a.unmousemove(e).unmouseup(l);return this}});C.plugin(function(a,v,y,C){y=y.prototype;var A=/^\s*url\((.+)\)/,w=String,z=a._.$;a.filter={};y.filter=function(d){var f=this;"svg"!=f.type&&(f=f.paper);d=a.parse(w(d));var k=a._.id(),u=z("filter");z(u,{id:k,filterUnits:"userSpaceOnUse"});u.appendChild(d.node);f.defs.appendChild(u);return new v(u)};k.on("snap.util.getattr.filter",
function(){k.stop();var d=z(this.node,"filter");if(d)return(d=w(d).match(A))&&a.select(d[1])});k.on("snap.util.attr.filter",function(d){if(d instanceof v&&"filter"==d.type){k.stop();var f=d.node.id;f||(z(d.node,{id:d.id}),f=d.id);z(this.node,{filter:a.url(f)})}d&&"none"!=d||(k.stop(),this.node.removeAttribute("filter"))});a.filter.blur=function(d,f){null==d&&(d=2);return a.format('<feGaussianBlur stdDeviation="{def}"/>',{def:null==f?d:[d,f]})};a.filter.blur.toString=function(){return this()};a.filter.shadow=
function(d,f,k,u,p){"string"==typeof k&&(p=u=k,k=4);"string"!=typeof u&&(p=u,u="#000");null==k&&(k=4);null==p&&(p=1);null==d&&(d=0,f=2);null==f&&(f=d);u=a.color(u||"#000");return a.format('<feGaussianBlur in="SourceAlpha" stdDeviation="{blur}"/><feOffset dx="{dx}" dy="{dy}" result="offsetblur"/><feFlood flood-color="{color}"/><feComposite in2="offsetblur" operator="in"/><feComponentTransfer><feFuncA type="linear" slope="{opacity}"/></feComponentTransfer><feMerge><feMergeNode/><feMergeNode in="SourceGraphic"/></feMerge>',
{color:u,dx:d,dy:f,blur:k,opacity:p})};a.filter.shadow.toString=function(){return this()};a.filter.grayscale=function(d){null==d&&(d=1);return a.format('<feColorMatrix type="matrix" values="{a} {b} {c} 0 0 {d} {e} {f} 0 0 {g} {b} {h} 0 0 0 0 0 1 0"/>',{a:0.2126+0.7874*(1-d),b:0.7152-0.7152*(1-d),c:0.0722-0.0722*(1-d),d:0.2126-0.2126*(1-d),e:0.7152+0.2848*(1-d),f:0.0722-0.0722*(1-d),g:0.2126-0.2126*(1-d),h:0.0722+0.9278*(1-d)})};a.filter.grayscale.toString=function(){return this()};a.filter.sepia=
function(d){null==d&&(d=1);return a.format('<feColorMatrix type="matrix" values="{a} {b} {c} 0 0 {d} {e} {f} 0 0 {g} {h} {i} 0 0 0 0 0 1 0"/>',{a:0.393+0.607*(1-d),b:0.769-0.769*(1-d),c:0.189-0.189*(1-d),d:0.349-0.349*(1-d),e:0.686+0.314*(1-d),f:0.168-0.168*(1-d),g:0.272-0.272*(1-d),h:0.534-0.534*(1-d),i:0.131+0.869*(1-d)})};a.filter.sepia.toString=function(){return this()};a.filter.saturate=function(d){null==d&&(d=1);return a.format('<feColorMatrix type="saturate" values="{amount}"/>',{amount:1-
d})};a.filter.saturate.toString=function(){return this()};a.filter.hueRotate=function(d){return a.format('<feColorMatrix type="hueRotate" values="{angle}"/>',{angle:d||0})};a.filter.hueRotate.toString=function(){return this()};a.filter.invert=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="table" tableValues="{amount} {amount2}"/><feFuncG type="table" tableValues="{amount} {amount2}"/><feFuncB type="table" tableValues="{amount} {amount2}"/></feComponentTransfer>',{amount:d,
amount2:1-d})};a.filter.invert.toString=function(){return this()};a.filter.brightness=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="linear" slope="{amount}"/><feFuncG type="linear" slope="{amount}"/><feFuncB type="linear" slope="{amount}"/></feComponentTransfer>',{amount:d})};a.filter.brightness.toString=function(){return this()};a.filter.contrast=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="linear" slope="{amount}" intercept="{amount2}"/><feFuncG type="linear" slope="{amount}" intercept="{amount2}"/><feFuncB type="linear" slope="{amount}" intercept="{amount2}"/></feComponentTransfer>',
{amount:d,amount2:0.5-d/2})};a.filter.contrast.toString=function(){return this()}});return C});

]]> </script>
<script> <![CDATA[

(function (glob, factory) {
    // AMD support
    if (typeof define === "function" && define.amd) {
        // Define as an anonymous module
        define("Gadfly", ["Snap.svg"], function (Snap) {
            return factory(Snap);
        });
    } else {
        // Browser globals (glob is window)
        // Snap adds itself to window
        glob.Gadfly = factory(glob.Snap);
    }
}(this, function (Snap) {

var Gadfly = {};

// Get an x/y coordinate value in pixels
var xPX = function(fig, x) {
    var client_box = fig.node.getBoundingClientRect();
    return x * fig.node.viewBox.baseVal.width / client_box.width;
};

var yPX = function(fig, y) {
    var client_box = fig.node.getBoundingClientRect();
    return y * fig.node.viewBox.baseVal.height / client_box.height;
};


Snap.plugin(function (Snap, Element, Paper, global) {
    // Traverse upwards from a snap element to find and return the first
    // note with the "plotroot" class.
    Element.prototype.plotroot = function () {
        var element = this;
        while (!element.hasClass("plotroot") && element.parent() != null) {
            element = element.parent();
        }
        return element;
    };

    Element.prototype.svgroot = function () {
        var element = this;
        while (element.node.nodeName != "svg" && element.parent() != null) {
            element = element.parent();
        }
        return element;
    };

    Element.prototype.plotbounds = function () {
        var root = this.plotroot()
        var bbox = root.select(".guide.background").node.getBBox();
        return {
            x0: bbox.x,
            x1: bbox.x + bbox.width,
            y0: bbox.y,
            y1: bbox.y + bbox.height
        };
    };

    Element.prototype.plotcenter = function () {
        var root = this.plotroot()
        var bbox = root.select(".guide.background").node.getBBox();
        return {
            x: bbox.x + bbox.width / 2,
            y: bbox.y + bbox.height / 2
        };
    };

    // Emulate IE style mouseenter/mouseleave events, since Microsoft always
    // does everything right.
    // See: http://www.dynamic-tools.net/toolbox/isMouseLeaveOrEnter/
    var events = ["mouseenter", "mouseleave"];

    for (i in events) {
        (function (event_name) {
            var event_name = events[i];
            Element.prototype[event_name] = function (fn, scope) {
                if (Snap.is(fn, "function")) {
                    var fn2 = function (event) {
                        if (event.type != "mouseover" && event.type != "mouseout") {
                            return;
                        }

                        var reltg = event.relatedTarget ? event.relatedTarget :
                            event.type == "mouseout" ? event.toElement : event.fromElement;
                        while (reltg && reltg != this.node) reltg = reltg.parentNode;

                        if (reltg != this.node) {
                            return fn.apply(this, event);
                        }
                    };

                    if (event_name == "mouseenter") {
                        this.mouseover(fn2, scope);
                    } else {
                        this.mouseout(fn2, scope);
                    }
                }
                return this;
            };
        })(events[i]);
    }


    Element.prototype.mousewheel = function (fn, scope) {
        if (Snap.is(fn, "function")) {
            var el = this;
            var fn2 = function (event) {
                fn.apply(el, [event]);
            };
        }

        this.node.addEventListener(
            /Firefox/i.test(navigator.userAgent) ? "DOMMouseScroll" : "mousewheel",
            fn2);

        return this;
    };


    // Snap's attr function can be too slow for things like panning/zooming.
    // This is a function to directly update element attributes without going
    // through eve.
    Element.prototype.attribute = function(key, val) {
        if (val === undefined) {
            return this.node.getAttribute(key);
        } else {
            this.node.setAttribute(key, val);
            return this;
        }
    };

    Element.prototype.init_gadfly = function() {
        this.mouseenter(Gadfly.plot_mouseover)
            .mouseleave(Gadfly.plot_mouseout)
            .dblclick(Gadfly.plot_dblclick)
            .mousewheel(Gadfly.guide_background_scroll)
            .drag(Gadfly.guide_background_drag_onmove,
                  Gadfly.guide_background_drag_onstart,
                  Gadfly.guide_background_drag_onend);
        this.mouseenter(function (event) {
            init_pan_zoom(this.plotroot());
        });
        return this;
    };
});


// When the plot is moused over, emphasize the grid lines.
Gadfly.plot_mouseover = function(event) {
    var root = this.plotroot();

    var keyboard_zoom = function(event) {
        if (event.which == 187) { // plus
            increase_zoom_by_position(root, 0.1, true);
        } else if (event.which == 189) { // minus
            increase_zoom_by_position(root, -0.1, true);
        }
    };
    root.data("keyboard_zoom", keyboard_zoom);
    window.addEventListener("keyup", keyboard_zoom);

    var xgridlines = root.select(".xgridlines"),
        ygridlines = root.select(".ygridlines");

    xgridlines.data("unfocused_strokedash",
                    xgridlines.attribute("stroke-dasharray").replace(/(\d)(,|$)/g, "$1mm$2"));
    ygridlines.data("unfocused_strokedash",
                    ygridlines.attribute("stroke-dasharray").replace(/(\d)(,|$)/g, "$1mm$2"));

    // emphasize grid lines
    var destcolor = root.data("focused_xgrid_color");
    xgridlines.attribute("stroke-dasharray", "none")
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    destcolor = root.data("focused_ygrid_color");
    ygridlines.attribute("stroke-dasharray", "none")
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    // reveal zoom slider
    root.select(".zoomslider")
        .animate({opacity: 1.0}, 250);
};

// Reset pan and zoom on double click
Gadfly.plot_dblclick = function(event) {
  set_plot_pan_zoom(this.plotroot(), 0.0, 0.0, 1.0);
};

// Unemphasize grid lines on mouse out.
Gadfly.plot_mouseout = function(event) {
    var root = this.plotroot();

    window.removeEventListener("keyup", root.data("keyboard_zoom"));
    root.data("keyboard_zoom", undefined);

    var xgridlines = root.select(".xgridlines"),
        ygridlines = root.select(".ygridlines");

    var destcolor = root.data("unfocused_xgrid_color");

    xgridlines.attribute("stroke-dasharray", xgridlines.data("unfocused_strokedash"))
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    destcolor = root.data("unfocused_ygrid_color");
    ygridlines.attribute("stroke-dasharray", ygridlines.data("unfocused_strokedash"))
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    // hide zoom slider
    root.select(".zoomslider")
        .animate({opacity: 0.0}, 250);
};


var set_geometry_transform = function(root, tx, ty, scale) {
    var xscalable = root.hasClass("xscalable"),
        yscalable = root.hasClass("yscalable");

    var old_scale = root.data("scale");

    var xscale = xscalable ? scale : 1.0,
        yscale = yscalable ? scale : 1.0;

    tx = xscalable ? tx : 0.0;
    ty = yscalable ? ty : 0.0;

    var t = new Snap.Matrix().translate(tx, ty).scale(xscale, yscale);

    root.selectAll(".geometry, image")
        .forEach(function (element, i) {
            element.transform(t);
        });

    bounds = root.plotbounds();

    if (yscalable) {
        var xfixed_t = new Snap.Matrix().translate(0, ty).scale(1.0, yscale);
        root.selectAll(".xfixed")
            .forEach(function (element, i) {
                element.transform(xfixed_t);
            });

        root.select(".ylabels")
            .transform(xfixed_t)
            .selectAll("text")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var cx = element.asPX("x"),
                        cy = element.asPX("y");
                    var st = element.data("static_transform");
                    unscale_t = new Snap.Matrix();
                    unscale_t.scale(1, 1/scale, cx, cy).add(st);
                    element.transform(unscale_t);

                    var y = cy * scale + ty;
                    element.attr("visibility",
                        bounds.y0 <= y && y <= bounds.y1 ? "visible" : "hidden");
                }
            });
    }

    if (xscalable) {
        var yfixed_t = new Snap.Matrix().translate(tx, 0).scale(xscale, 1.0);
        var xtrans = new Snap.Matrix().translate(tx, 0);
        root.selectAll(".yfixed")
            .forEach(function (element, i) {
                element.transform(yfixed_t);
            });

        root.select(".xlabels")
            .transform(yfixed_t)
            .selectAll("text")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var cx = element.asPX("x"),
                        cy = element.asPX("y");
                    var st = element.data("static_transform");
                    unscale_t = new Snap.Matrix();
                    unscale_t.scale(1/scale, 1, cx, cy).add(st);

                    element.transform(unscale_t);

                    var x = cx * scale + tx;
                    element.attr("visibility",
                        bounds.x0 <= x && x <= bounds.x1 ? "visible" : "hidden");
                    }
            });
    }

    // we must unscale anything that is scale invariance: widths, raiduses, etc.
    var size_attribs = ["font-size"];
    var unscaled_selection = ".geometry, .geometry *";
    if (xscalable) {
        size_attribs.push("rx");
        unscaled_selection += ", .xgridlines";
    }
    if (yscalable) {
        size_attribs.push("ry");
        unscaled_selection += ", .ygridlines";
    }

    root.selectAll(unscaled_selection)
        .forEach(function (element, i) {
            // circle need special help
            if (element.node.nodeName == "circle") {
                var cx = element.attribute("cx"),
                    cy = element.attribute("cy");
                unscale_t = new Snap.Matrix().scale(1/xscale, 1/yscale,
                                                        cx, cy);
                element.transform(unscale_t);
                return;
            }

            for (i in size_attribs) {
                var key = size_attribs[i];
                var val = parseFloat(element.attribute(key));
                if (val !== undefined && val != 0 && !isNaN(val)) {
                    element.attribute(key, val * old_scale / scale);
                }
            }
        });
};


// Find the most appropriate tick scale and update label visibility.
var update_tickscale = function(root, scale, axis) {
    if (!root.hasClass(axis + "scalable")) return;

    var tickscales = root.data(axis + "tickscales");
    var best_tickscale = 1.0;
    var best_tickscale_dist = Infinity;
    for (tickscale in tickscales) {
        var dist = Math.abs(Math.log(tickscale) - Math.log(scale));
        if (dist < best_tickscale_dist) {
            best_tickscale_dist = dist;
            best_tickscale = tickscale;
        }
    }

    if (best_tickscale != root.data(axis + "tickscale")) {
        root.data(axis + "tickscale", best_tickscale);
        var mark_inscale_gridlines = function (element, i) {
            var inscale = element.attr("gadfly:scale") == best_tickscale;
            element.attribute("gadfly:inscale", inscale);
            element.attr("visibility", inscale ? "visible" : "hidden");
        };

        var mark_inscale_labels = function (element, i) {
            var inscale = element.attr("gadfly:scale") == best_tickscale;
            element.attribute("gadfly:inscale", inscale);
            element.attr("visibility", inscale ? "visible" : "hidden");
        };

        root.select("." + axis + "gridlines").selectAll("path").forEach(mark_inscale_gridlines);
        root.select("." + axis + "labels").selectAll("text").forEach(mark_inscale_labels);
    }
};


var set_plot_pan_zoom = function(root, tx, ty, scale) {
    var old_scale = root.data("scale");
    var bounds = root.plotbounds();

    var width = bounds.x1 - bounds.x0,
        height = bounds.y1 - bounds.y0;

    // compute the viewport derived from tx, ty, and scale
    var x_min = -width * scale - (scale * width - width),
        x_max = width * scale,
        y_min = -height * scale - (scale * height - height),
        y_max = height * scale;

    var x0 = bounds.x0 - scale * bounds.x0,
        y0 = bounds.y0 - scale * bounds.y0;

    var tx = Math.max(Math.min(tx - x0, x_max), x_min),
        ty = Math.max(Math.min(ty - y0, y_max), y_min);

    tx += x0;
    ty += y0;

    // when the scale change, we may need to alter which set of
    // ticks is being displayed
    if (scale != old_scale) {
        update_tickscale(root, scale, "x");
        update_tickscale(root, scale, "y");
    }

    set_geometry_transform(root, tx, ty, scale);

    root.data("scale", scale);
    root.data("tx", tx);
    root.data("ty", ty);
};


var scale_centered_translation = function(root, scale) {
    var bounds = root.plotbounds();

    var width = bounds.x1 - bounds.x0,
        height = bounds.y1 - bounds.y0;

    var tx0 = root.data("tx"),
        ty0 = root.data("ty");

    var scale0 = root.data("scale");

    // how off from center the current view is
    var xoff = tx0 - (bounds.x0 * (1 - scale0) + (width * (1 - scale0)) / 2),
        yoff = ty0 - (bounds.y0 * (1 - scale0) + (height * (1 - scale0)) / 2);

    // rescale offsets
    xoff = xoff * scale / scale0;
    yoff = yoff * scale / scale0;

    // adjust for the panel position being scaled
    var x_edge_adjust = bounds.x0 * (1 - scale),
        y_edge_adjust = bounds.y0 * (1 - scale);

    return {
        x: xoff + x_edge_adjust + (width - width * scale) / 2,
        y: yoff + y_edge_adjust + (height - height * scale) / 2
    };
};


// Initialize data for panning zooming if it isn't already.
var init_pan_zoom = function(root) {
    if (root.data("zoompan-ready")) {
        return;
    }

    // The non-scaling-stroke trick. Rather than try to correct for the
    // stroke-width when zooming, we force it to a fixed value.
    var px_per_mm = root.node.getCTM().a;

    // Drag events report deltas in pixels, which we'd like to convert to
    // millimeters.
    root.data("px_per_mm", px_per_mm);

    root.selectAll("path")
        .forEach(function (element, i) {
        sw = element.asPX("stroke-width") * px_per_mm;
        if (sw > 0) {
            element.attribute("stroke-width", sw);
            element.attribute("vector-effect", "non-scaling-stroke");
        }
    });

    // Store ticks labels original tranformation
    root.selectAll(".xlabels > text, .ylabels > text")
        .forEach(function (element, i) {
            var lm = element.transform().localMatrix;
            element.data("static_transform",
                new Snap.Matrix(lm.a, lm.b, lm.c, lm.d, lm.e, lm.f));
        });

    var xgridlines = root.select(".xgridlines");
    var ygridlines = root.select(".ygridlines");
    var xlabels = root.select(".xlabels");
    var ylabels = root.select(".ylabels");

    if (root.data("tx") === undefined) root.data("tx", 0);
    if (root.data("ty") === undefined) root.data("ty", 0);
    if (root.data("scale") === undefined) root.data("scale", 1.0);
    if (root.data("xtickscales") === undefined) {

        // index all the tick scales that are listed
        var xtickscales = {};
        var ytickscales = {};
        var add_x_tick_scales = function (element, i) {
            xtickscales[element.attribute("gadfly:scale")] = true;
        };
        var add_y_tick_scales = function (element, i) {
            ytickscales[element.attribute("gadfly:scale")] = true;
        };

        if (xgridlines) xgridlines.selectAll("path").forEach(add_x_tick_scales);
        if (ygridlines) ygridlines.selectAll("path").forEach(add_y_tick_scales);
        if (xlabels) xlabels.selectAll("text").forEach(add_x_tick_scales);
        if (ylabels) ylabels.selectAll("text").forEach(add_y_tick_scales);

        root.data("xtickscales", xtickscales);
        root.data("ytickscales", ytickscales);
        root.data("xtickscale", 1.0);
    }

    var min_scale = 1.0, max_scale = 1.0;
    for (scale in xtickscales) {
        min_scale = Math.min(min_scale, scale);
        max_scale = Math.max(max_scale, scale);
    }
    for (scale in ytickscales) {
        min_scale = Math.min(min_scale, scale);
        max_scale = Math.max(max_scale, scale);
    }
    root.data("min_scale", min_scale);
    root.data("max_scale", max_scale);

    // store the original positions of labels
    if (xlabels) {
        xlabels.selectAll("text")
               .forEach(function (element, i) {
                   element.data("x", element.asPX("x"));
               });
    }

    if (ylabels) {
        ylabels.selectAll("text")
               .forEach(function (element, i) {
                   element.data("y", element.asPX("y"));
               });
    }

    // mark grid lines and ticks as in or out of scale.
    var mark_inscale = function (element, i) {
        element.attribute("gadfly:inscale", element.attribute("gadfly:scale") == 1.0);
    };

    if (xgridlines) xgridlines.selectAll("path").forEach(mark_inscale);
    if (ygridlines) ygridlines.selectAll("path").forEach(mark_inscale);
    if (xlabels) xlabels.selectAll("text").forEach(mark_inscale);
    if (ylabels) ylabels.selectAll("text").forEach(mark_inscale);

    // figure out the upper ond lower bounds on panning using the maximum
    // and minum grid lines
    var bounds = root.plotbounds();
    var pan_bounds = {
        x0: 0.0,
        y0: 0.0,
        x1: 0.0,
        y1: 0.0
    };

    if (xgridlines) {
        xgridlines
            .selectAll("path")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var bbox = element.node.getBBox();
                    if (bounds.x1 - bbox.x < pan_bounds.x0) {
                        pan_bounds.x0 = bounds.x1 - bbox.x;
                    }
                    if (bounds.x0 - bbox.x > pan_bounds.x1) {
                        pan_bounds.x1 = bounds.x0 - bbox.x;
                    }
                    element.attr("visibility", "visible");
                }
            });
    }

    if (ygridlines) {
        ygridlines
            .selectAll("path")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var bbox = element.node.getBBox();
                    if (bounds.y1 - bbox.y < pan_bounds.y0) {
                        pan_bounds.y0 = bounds.y1 - bbox.y;
                    }
                    if (bounds.y0 - bbox.y > pan_bounds.y1) {
                        pan_bounds.y1 = bounds.y0 - bbox.y;
                    }
                    element.attr("visibility", "visible");
                }
            });
    }

    // nudge these values a little
    pan_bounds.x0 -= 5;
    pan_bounds.x1 += 5;
    pan_bounds.y0 -= 5;
    pan_bounds.y1 += 5;
    root.data("pan_bounds", pan_bounds);

    root.data("zoompan-ready", true)
};


// drag actions, i.e. zooming and panning
var pan_action = {
    start: function(root, x, y, event) {
        root.data("dx", 0);
        root.data("dy", 0);
        root.data("tx0", root.data("tx"));
        root.data("ty0", root.data("ty"));
    },
    update: function(root, dx, dy, x, y, event) {
        var px_per_mm = root.data("px_per_mm");
        dx /= px_per_mm;
        dy /= px_per_mm;

        var tx0 = root.data("tx"),
            ty0 = root.data("ty");

        var dx0 = root.data("dx"),
            dy0 = root.data("dy");

        root.data("dx", dx);
        root.data("dy", dy);

        dx = dx - dx0;
        dy = dy - dy0;

        var tx = tx0 + dx,
            ty = ty0 + dy;

        set_plot_pan_zoom(root, tx, ty, root.data("scale"));
    },
    end: function(root, event) {

    },
    cancel: function(root) {
        set_plot_pan_zoom(root, root.data("tx0"), root.data("ty0"), root.data("scale"));
    }
};

var zoom_box;
var zoom_action = {
    start: function(root, x, y, event) {
        var bounds = root.plotbounds();
        var width = bounds.x1 - bounds.x0,
            height = bounds.y1 - bounds.y0;
        var ratio = width / height;
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var px_per_mm = root.data("px_per_mm");
        x = xscalable ? x / px_per_mm : bounds.x0;
        y = yscalable ? y / px_per_mm : bounds.y0;
        var w = xscalable ? 0 : width;
        var h = yscalable ? 0 : height;
        zoom_box = root.rect(x, y, w, h).attr({
            "fill": "#000",
            "opacity": 0.25
        });
        zoom_box.data("ratio", ratio);
    },
    update: function(root, dx, dy, x, y, event) {
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var px_per_mm = root.data("px_per_mm");
        var bounds = root.plotbounds();
        if (yscalable) {
            y /= px_per_mm;
            y = Math.max(bounds.y0, y);
            y = Math.min(bounds.y1, y);
        } else {
            y = bounds.y1;
        }
        if (xscalable) {
            x /= px_per_mm;
            x = Math.max(bounds.x0, x);
            x = Math.min(bounds.x1, x);
        } else {
            x = bounds.x1;
        }

        dx = x - zoom_box.attr("x");
        dy = y - zoom_box.attr("y");
        if (xscalable && yscalable) {
            var ratio = zoom_box.data("ratio");
            var width = Math.min(Math.abs(dx), ratio * Math.abs(dy));
            var height = Math.min(Math.abs(dy), Math.abs(dx) / ratio);
            dx = width * dx / Math.abs(dx);
            dy = height * dy / Math.abs(dy);
        }
        var xoffset = 0,
            yoffset = 0;
        if (dx < 0) {
            xoffset = dx;
            dx = -1 * dx;
        }
        if (dy < 0) {
            yoffset = dy;
            dy = -1 * dy;
        }
        if (isNaN(dy)) {
            dy = 0.0;
        }
        if (isNaN(dx)) {
            dx = 0.0;
        }
        zoom_box.transform("T" + xoffset + "," + yoffset);
        zoom_box.attr("width", dx);
        zoom_box.attr("height", dy);
    },
    end: function(root, event) {
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var zoom_bounds = zoom_box.getBBox();
        if (zoom_bounds.width * zoom_bounds.height <= 0) {
            return;
        }
        var plot_bounds = root.plotbounds();
        var zoom_factor = 1.0;
        if (yscalable) {
            zoom_factor = (plot_bounds.y1 - plot_bounds.y0) / zoom_bounds.height;
        } else {
            zoom_factor = (plot_bounds.x1 - plot_bounds.x0) / zoom_bounds.width;
        }
        var tx = (root.data("tx") - zoom_bounds.x) * zoom_factor + plot_bounds.x0,
            ty = (root.data("ty") - zoom_bounds.y) * zoom_factor + plot_bounds.y0;
        set_plot_pan_zoom(root, tx, ty, root.data("scale") * zoom_factor);
        zoom_box.remove();
    },
    cancel: function(root) {
        zoom_box.remove();
    }
};


Gadfly.guide_background_drag_onstart = function(x, y, event) {
    var root = this.plotroot();
    var scalable = root.hasClass("xscalable") || root.hasClass("yscalable");
    var zoomable = !event.altKey && !event.ctrlKey && event.shiftKey && scalable;
    var panable = !event.altKey && !event.ctrlKey && !event.shiftKey && scalable;
    var drag_action = zoomable ? zoom_action :
                      panable  ? pan_action :
                                 undefined;
    root.data("drag_action", drag_action);
    if (drag_action) {
        var cancel_drag_action = function(event) {
            if (event.which == 27) { // esc key
                drag_action.cancel(root);
                root.data("drag_action", undefined);
            }
        };
        window.addEventListener("keyup", cancel_drag_action);
        root.data("cancel_drag_action", cancel_drag_action);
        drag_action.start(root, x, y, event);
    }
};


Gadfly.guide_background_drag_onmove = function(dx, dy, x, y, event) {
    var root = this.plotroot();
    var drag_action = root.data("drag_action");
    if (drag_action) {
        drag_action.update(root, dx, dy, x, y, event);
    }
};


Gadfly.guide_background_drag_onend = function(event) {
    var root = this.plotroot();
    window.removeEventListener("keyup", root.data("cancel_drag_action"));
    root.data("cancel_drag_action", undefined);
    var drag_action = root.data("drag_action");
    if (drag_action) {
        drag_action.end(root, event);
    }
    root.data("drag_action", undefined);
};


Gadfly.guide_background_scroll = function(event) {
    if (event.shiftKey) {
        increase_zoom_by_position(this.plotroot(), 0.001 * event.wheelDelta);
        event.preventDefault();
    }
};


Gadfly.zoomslider_button_mouseover = function(event) {
    this.select(".button_logo")
         .animate({fill: this.data("mouseover_color")}, 100);
};


Gadfly.zoomslider_button_mouseout = function(event) {
     this.select(".button_logo")
         .animate({fill: this.data("mouseout_color")}, 100);
};


Gadfly.zoomslider_zoomout_click = function(event) {
    increase_zoom_by_position(this.plotroot(), -0.1, true);
};


Gadfly.zoomslider_zoomin_click = function(event) {
    increase_zoom_by_position(this.plotroot(), 0.1, true);
};


Gadfly.zoomslider_track_click = function(event) {
    // TODO
};


// Map slider position x to scale y using the function y = a*exp(b*x)+c.
// The constants a, b, and c are solved using the constraint that the function
// should go through the points (0; min_scale), (0.5; 1), and (1; max_scale).
var scale_from_slider_position = function(position, min_scale, max_scale) {
    var a = (1 - 2 * min_scale + min_scale * min_scale) / (min_scale + max_scale - 2),
        b = 2 * Math.log((max_scale - 1) / (1 - min_scale)),
        c = (min_scale * max_scale - 1) / (min_scale + max_scale - 2);
    return a * Math.exp(b * position) + c;
}

// inverse of scale_from_slider_position
var slider_position_from_scale = function(scale, min_scale, max_scale) {
    var a = (1 - 2 * min_scale + min_scale * min_scale) / (min_scale + max_scale - 2),
        b = 2 * Math.log((max_scale - 1) / (1 - min_scale)),
        c = (min_scale * max_scale - 1) / (min_scale + max_scale - 2);
    return 1 / b * Math.log((scale - c) / a);
}

var increase_zoom_by_position = function(root, delta_position, animate) {
    var scale = root.data("scale"),
        min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale");
    var position = slider_position_from_scale(scale, min_scale, max_scale);
    position += delta_position;
    scale = scale_from_slider_position(position, min_scale, max_scale);
    set_zoom(root, scale, animate);
}

var set_zoom = function(root, scale, animate) {
    var min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale"),
        old_scale = root.data("scale");
    var new_scale = Math.max(min_scale, Math.min(scale, max_scale));
    if (animate) {
        Snap.animate(
            old_scale,
            new_scale,
            function (new_scale) {
                update_plot_scale(root, new_scale);
            },
            200);
    } else {
        update_plot_scale(root, new_scale);
    }
}


var update_plot_scale = function(root, new_scale) {
    var trans = scale_centered_translation(root, new_scale);
    set_plot_pan_zoom(root, trans.x, trans.y, new_scale);

    root.selectAll(".zoomslider_thumb")
        .forEach(function (element, i) {
            var min_pos = element.data("min_pos"),
                max_pos = element.data("max_pos"),
                min_scale = root.data("min_scale"),
                max_scale = root.data("max_scale");
            var xmid = (min_pos + max_pos) / 2;
            var xpos = slider_position_from_scale(new_scale, min_scale, max_scale);
            element.transform(new Snap.Matrix().translate(
                Math.max(min_pos, Math.min(
                         max_pos, min_pos + (max_pos - min_pos) * xpos)) - xmid, 0));
    });
};


Gadfly.zoomslider_thumb_dragmove = function(dx, dy, x, y, event) {
    var root = this.plotroot();
    var min_pos = this.data("min_pos"),
        max_pos = this.data("max_pos"),
        min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale"),
        old_scale = root.data("old_scale");

    var px_per_mm = root.data("px_per_mm");
    dx /= px_per_mm;
    dy /= px_per_mm;

    var xmid = (min_pos + max_pos) / 2;
    var xpos = slider_position_from_scale(old_scale, min_scale, max_scale) +
                   dx / (max_pos - min_pos);

    // compute the new scale
    var new_scale = scale_from_slider_position(xpos, min_scale, max_scale);
    new_scale = Math.min(max_scale, Math.max(min_scale, new_scale));

    update_plot_scale(root, new_scale);
    event.stopPropagation();
};


Gadfly.zoomslider_thumb_dragstart = function(x, y, event) {
    this.animate({fill: this.data("mouseover_color")}, 100);
    var root = this.plotroot();

    // keep track of what the scale was when we started dragging
    root.data("old_scale", root.data("scale"));
    event.stopPropagation();
};


Gadfly.zoomslider_thumb_dragend = function(event) {
    this.animate({fill: this.data("mouseout_color")}, 100);
    event.stopPropagation();
};


var toggle_color_class = function(root, color_class, ison) {
    var guides = root.selectAll(".guide." + color_class + ",.guide ." + color_class);
    var geoms = root.selectAll(".geometry." + color_class + ",.geometry ." + color_class);
    if (ison) {
        guides.animate({opacity: 0.5}, 250);
        geoms.animate({opacity: 0.0}, 250);
    } else {
        guides.animate({opacity: 1.0}, 250);
        geoms.animate({opacity: 1.0}, 250);
    }
};


Gadfly.colorkey_swatch_click = function(event) {
    var root = this.plotroot();
    var color_class = this.data("color_class");

    if (event.shiftKey) {
        root.selectAll(".colorkey text")
            .forEach(function (element) {
                var other_color_class = element.data("color_class");
                if (other_color_class != color_class) {
                    toggle_color_class(root, other_color_class,
                                       element.attr("opacity") == 1.0);
                }
            });
    } else {
        toggle_color_class(root, color_class, this.attr("opacity") == 1.0);
    }
};


return Gadfly;

}));


//@ sourceURL=gadfly.js

(function (glob, factory) {
    // AMD support
      if (typeof require === "function" && typeof define === "function" && define.amd) {
        require(["Snap.svg", "Gadfly"], function (Snap, Gadfly) {
            factory(Snap, Gadfly);
        });
      } else {
          factory(glob.Snap, glob.Gadfly);
      }
})(window, function (Snap, Gadfly) {
    var fig = Snap("#img-91c8bd3d");
fig.select("#img-91c8bd3d-4")
   .drag(function() {}, function() {}, function() {});
fig.select("#img-91c8bd3d-6")
   .data("color_class", "color_Emission")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-91c8bd3d-7")
   .data("color_class", "color_SoilMoisture")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-91c8bd3d-8")
   .data("color_class", "color_t2m")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-91c8bd3d-10")
   .data("color_class", "color_Emission")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-91c8bd3d-11")
   .data("color_class", "color_SoilMoisture")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-91c8bd3d-12")
   .data("color_class", "color_t2m")
.click(Gadfly.colorkey_swatch_click)
;
fig.select("#img-91c8bd3d-16")
   .init_gadfly();
fig.select("#img-91c8bd3d-19")
   .plotroot().data("unfocused_ygrid_color", "#D0D0E0")
;
fig.select("#img-91c8bd3d-19")
   .plotroot().data("focused_ygrid_color", "#A0A0A0")
;
fig.select("#img-91c8bd3d-158")
   .plotroot().data("unfocused_xgrid_color", "#D0D0E0")
;
fig.select("#img-91c8bd3d-158")
   .plotroot().data("focused_xgrid_color", "#A0A0A0")
;
fig.select("#img-91c8bd3d-197")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-91c8bd3d-197")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-91c8bd3d-197")
   .click(Gadfly.zoomslider_zoomin_click)
.mouseenter(Gadfly.zoomslider_button_mouseover)
.mouseleave(Gadfly.zoomslider_button_mouseout)
;
fig.select("#img-91c8bd3d-201")
   .data("max_pos", 101.45)
;
fig.select("#img-91c8bd3d-201")
   .data("min_pos", 84.45)
;
fig.select("#img-91c8bd3d-201")
   .click(Gadfly.zoomslider_track_click);
fig.select("#img-91c8bd3d-203")
   .data("max_pos", 101.45)
;
fig.select("#img-91c8bd3d-203")
   .data("min_pos", 84.45)
;
fig.select("#img-91c8bd3d-203")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-91c8bd3d-203")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-91c8bd3d-203")
   .drag(Gadfly.zoomslider_thumb_dragmove,
     Gadfly.zoomslider_thumb_dragstart,
     Gadfly.zoomslider_thumb_dragend)
;
fig.select("#img-91c8bd3d-205")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-91c8bd3d-205")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-91c8bd3d-205")
   .click(Gadfly.zoomslider_zoomout_click)
.mouseenter(Gadfly.zoomslider_button_mouseover)
.mouseleave(Gadfly.zoomslider_button_mouseout)
;
    });
]]> </script>
</svg>




```julia
plotTS(scores)
```










<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     xmlns:gadfly="http://www.gadflyjl.org/ns"
     version="1.2"
     width="141.42mm" height="100mm" viewBox="0 0 141.42 100"
     stroke="none"
     fill="#000000"
     stroke-width="0.3"
     font-size="3.88"

     id="img-e512d6eb">
<g class="plotroot xscalable yscalable" id="img-e512d6eb-1">
  <g font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" fill="#564A55" stroke="#000000" stroke-opacity="0.000" id="img-e512d6eb-2">
    <text x="77.12" y="88.39" text-anchor="middle" dy="0.6em">x</text>
  </g>
  <g class="guide xlabels" font-size="2.82" font-family="'PT Sans Caption','Helvetica Neue','Helvetica',sans-serif" fill="#6C606B" id="img-e512d6eb-3">
    <text x="-132.96" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">Jan 1, 1980</text>
    <text x="-94.75" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1985</text>
    <text x="-56.56" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1990</text>
    <text x="-18.37" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">1995</text>
    <text x="19.83" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2000</text>
    <text x="58.04" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2005</text>
    <text x="96.23" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2010</text>
    <text x="134.42" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="visible">2015</text>
    <text x="172.61" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2020</text>
    <text x="210.83" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2025</text>
    <text x="249.02" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2030</text>
    <text x="287.21" y="84.39" text-anchor="middle" gadfly:scale="1.0" visibility="hidden">2035</text>
    <text x="-132.96" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">Jan 1, 1980</text>
    <text x="-56.56" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">1990</text>
    <text x="19.83" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2000</text>
    <text x="96.23" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2010</text>
    <text x="172.61" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2020</text>
    <text x="249.02" y="84.39" text-anchor="middle" gadfly:scale="2.2829166666666665" visibility="hidden">2030</text>
    <text x="-132.96" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">Jan 1, 1980</text>
    <text x="-56.56" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">1990</text>
    <text x="19.83" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2000</text>
    <text x="96.23" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2010</text>
    <text x="172.61" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2020</text>
    <text x="249.02" y="84.39" text-anchor="middle" gadfly:scale="821.85" visibility="hidden">2030</text>
    <text x="-132.96" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">Jan 1, 1980</text>
    <text x="-56.56" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">1990</text>
    <text x="19.83" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2000</text>
    <text x="96.23" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2010</text>
    <text x="172.61" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2020</text>
    <text x="249.02" y="84.39" text-anchor="middle" gadfly:scale="9.131666666666666" visibility="hidden">2030</text>
  </g>
<g clip-path="url(#img-e512d6eb-4)">
  <g id="img-e512d6eb-5">
    <g pointer-events="visible" opacity="1" fill="#000000" fill-opacity="0.000" stroke="#000000" stroke-opacity="0.000" class="guide background" id="img-e512d6eb-6">
      <rect x="17.83" y="5" width="118.6" height="75.72"/>
    </g>
    <g class="guide ygridlines xfixed" stroke-dasharray="0.5,0.5" stroke-width="0.2" stroke="#D0D0E0" id="img-e512d6eb-7">
      <path fill="none" d="M17.83,164.77 L 136.42 164.77" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,150.43 L 136.42 150.43" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,136.09 L 136.42 136.09" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,121.74 L 136.42 121.74" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,107.4 L 136.42 107.4" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,93.06 L 136.42 93.06" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,78.72 L 136.42 78.72" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.83,64.37 L 136.42 64.37" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.83,50.03 L 136.42 50.03" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.83,35.69 L 136.42 35.69" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.83,21.34 L 136.42 21.34" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.83,7 L 136.42 7" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M17.83,-7.34 L 136.42 -7.34" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-21.69 L 136.42 -21.69" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-36.03 L 136.42 -36.03" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-50.37 L 136.42 -50.37" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-64.72 L 136.42 -64.72" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-79.06 L 136.42 -79.06" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M17.83,150.43 L 136.42 150.43" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,147.56 L 136.42 147.56" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,144.69 L 136.42 144.69" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,141.82 L 136.42 141.82" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,138.96 L 136.42 138.96" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,136.09 L 136.42 136.09" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,133.22 L 136.42 133.22" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,130.35 L 136.42 130.35" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,127.48 L 136.42 127.48" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,124.61 L 136.42 124.61" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,121.74 L 136.42 121.74" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,118.88 L 136.42 118.88" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,116.01 L 136.42 116.01" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,113.14 L 136.42 113.14" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,110.27 L 136.42 110.27" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,107.4 L 136.42 107.4" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,104.53 L 136.42 104.53" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,101.66 L 136.42 101.66" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,98.8 L 136.42 98.8" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,95.93 L 136.42 95.93" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,93.06 L 136.42 93.06" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,90.19 L 136.42 90.19" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,87.32 L 136.42 87.32" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,84.45 L 136.42 84.45" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,81.58 L 136.42 81.58" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,78.72 L 136.42 78.72" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,75.85 L 136.42 75.85" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,72.98 L 136.42 72.98" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,70.11 L 136.42 70.11" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,67.24 L 136.42 67.24" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,64.37 L 136.42 64.37" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,61.5 L 136.42 61.5" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,58.63 L 136.42 58.63" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,55.77 L 136.42 55.77" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,52.9 L 136.42 52.9" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,50.03 L 136.42 50.03" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,47.16 L 136.42 47.16" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,44.29 L 136.42 44.29" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,41.42 L 136.42 41.42" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,38.55 L 136.42 38.55" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,35.69 L 136.42 35.69" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,32.82 L 136.42 32.82" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,29.95 L 136.42 29.95" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,27.08 L 136.42 27.08" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,24.21 L 136.42 24.21" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,21.34 L 136.42 21.34" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,18.47 L 136.42 18.47" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,15.61 L 136.42 15.61" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,12.74 L 136.42 12.74" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,9.87 L 136.42 9.87" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,7 L 136.42 7" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,4.13 L 136.42 4.13" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,1.26 L 136.42 1.26" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-1.61 L 136.42 -1.61" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-4.47 L 136.42 -4.47" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-7.34 L 136.42 -7.34" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-10.21 L 136.42 -10.21" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-13.08 L 136.42 -13.08" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-15.95 L 136.42 -15.95" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-18.82 L 136.42 -18.82" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-21.69 L 136.42 -21.69" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-24.55 L 136.42 -24.55" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-27.42 L 136.42 -27.42" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-30.29 L 136.42 -30.29" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-33.16 L 136.42 -33.16" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-36.03 L 136.42 -36.03" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-38.9 L 136.42 -38.9" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-41.77 L 136.42 -41.77" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-44.63 L 136.42 -44.63" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-47.5 L 136.42 -47.5" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-50.37 L 136.42 -50.37" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-53.24 L 136.42 -53.24" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-56.11 L 136.42 -56.11" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-58.98 L 136.42 -58.98" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-61.85 L 136.42 -61.85" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-64.72 L 136.42 -64.72" gadfly:scale="10.0" visibility="hidden"/>
      <path fill="none" d="M17.83,150.43 L 136.42 150.43" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.83,78.72 L 136.42 78.72" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.83,7 L 136.42 7" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.83,-64.72 L 136.42 -64.72" gadfly:scale="0.5" visibility="hidden"/>
      <path fill="none" d="M17.83,150.43 L 136.42 150.43" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,143.26 L 136.42 143.26" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,136.09 L 136.42 136.09" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,128.92 L 136.42 128.92" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,121.74 L 136.42 121.74" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,114.57 L 136.42 114.57" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,107.4 L 136.42 107.4" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,100.23 L 136.42 100.23" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,93.06 L 136.42 93.06" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,85.89 L 136.42 85.89" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,78.72 L 136.42 78.72" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,71.54 L 136.42 71.54" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,64.37 L 136.42 64.37" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,57.2 L 136.42 57.2" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,50.03 L 136.42 50.03" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,42.86 L 136.42 42.86" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,35.69 L 136.42 35.69" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,28.51 L 136.42 28.51" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,21.34 L 136.42 21.34" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,14.17 L 136.42 14.17" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,7 L 136.42 7" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-0.17 L 136.42 -0.17" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-7.34 L 136.42 -7.34" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-14.51 L 136.42 -14.51" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-21.69 L 136.42 -21.69" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-28.86 L 136.42 -28.86" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-36.03 L 136.42 -36.03" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-43.2 L 136.42 -43.2" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-50.37 L 136.42 -50.37" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-57.54 L 136.42 -57.54" gadfly:scale="5.0" visibility="hidden"/>
      <path fill="none" d="M17.83,-64.72 L 136.42 -64.72" gadfly:scale="5.0" visibility="hidden"/>
    </g>
    <g class="guide xgridlines yfixed" stroke-dasharray="0.5,0.5" stroke-width="0.2" stroke="#D0D0E0" id="img-e512d6eb-8">
      <path fill="none" d="M-132.96,5 L -132.96 80.72" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-94.75,5 L -94.75 80.72" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-56.56,5 L -56.56 80.72" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-18.37,5 L -18.37 80.72" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M19.83,5 L 19.83 80.72" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M58.04,5 L 58.04 80.72" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M96.23,5 L 96.23 80.72" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M134.42,5 L 134.42 80.72" gadfly:scale="1.0" visibility="visible"/>
      <path fill="none" d="M172.61,5 L 172.61 80.72" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M210.83,5 L 210.83 80.72" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M249.02,5 L 249.02 80.72" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M287.21,5 L 287.21 80.72" gadfly:scale="1.0" visibility="hidden"/>
      <path fill="none" d="M-132.96,5 L -132.96 80.72" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M-56.56,5 L -56.56 80.72" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M19.83,5 L 19.83 80.72" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M96.23,5 L 96.23 80.72" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M172.61,5 L 172.61 80.72" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M249.02,5 L 249.02 80.72" gadfly:scale="2.2829166666666665" visibility="hidden"/>
      <path fill="none" d="M-132.96,5 L -132.96 80.72" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M-56.56,5 L -56.56 80.72" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M19.83,5 L 19.83 80.72" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M96.23,5 L 96.23 80.72" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M172.61,5 L 172.61 80.72" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M249.02,5 L 249.02 80.72" gadfly:scale="821.85" visibility="hidden"/>
      <path fill="none" d="M-132.96,5 L -132.96 80.72" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M-56.56,5 L -56.56 80.72" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M19.83,5 L 19.83 80.72" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M96.23,5 L 96.23 80.72" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M172.61,5 L 172.61 80.72" gadfly:scale="9.131666666666666" visibility="hidden"/>
      <path fill="none" d="M249.02,5 L 249.02 80.72" gadfly:scale="9.131666666666666" visibility="hidden"/>
    </g>
    <g class="plotpanel" id="img-e512d6eb-9">
      <g stroke-width="0.3" fill="#000000" fill-opacity="0.000" class="geometry" stroke-dasharray="none" stroke="#00BFFF" id="img-e512d6eb-10">
        <path fill="none" d="M19.83,7.43 L 19.99 7.57 20.16 7.72 20.33 7.86 20.5 8 20.66 8.15 20.83 8.29 21 8.29 21.16 8.29 21.33 8.29 21.5 8.29 21.67 10.01 21.83 10.01 22 8.29 22.17 9.15 22.34 8.43 22.5 8.29 22.67 8.29 22.84 8.29 23 8.29 23.17 8.29 23.34 8.29 23.51 8.29 23.67 8.29 23.84 8.29 24.01 8.29 24.18 8.29 24.34 8.29 24.51 8.29 24.68 8.43 24.85 8.29 25.01 8.29 25.18 13.6 25.35 8.29 25.51 8.29 25.68 8.29 25.85 8.29 26.02 8.29 26.18 8.43 26.35 8.29 26.52 8.29 26.69 8.29 26.85 8.29 27.02 8.29 27.19 8.29 27.36 8.29 27.48 8.29 27.65 8.29 27.82 8.29 27.98 8.29 28.15 8.29 28.32 8.29 28.48 8.29 28.65 8.29 28.82 9.29 28.99 8.29 29.15 8.15 29.32 8.43 29.49 13.88 29.66 8.72 29.82 8.43 29.99 8.15 30.16 8.29 30.33 8.29 30.49 8.29 30.66 8.29 30.83 8.29 30.99 8.29 31.16 8.29 31.33 8.43 31.5 8.43 31.66 8.43 31.83 8.72 32 9.01 32.17 8.58 32.33 8.43 32.5 8.29 32.67 8.15 32.84 8.43 33 8.15 33.17 8.15 33.34 9.58 33.5 8.29 33.67 8.29 33.84 8.29 34.01 21.77 34.17 8.29 34.34 8.29 34.51 8.29 34.68 8.29 34.84 8.29 35.01 8.29 35.11 8.29 35.28 8.29 35.45 8.15 35.62 8.29 35.78 8.15 35.95 8.29 36.12 8.29 36.29 8.29 36.45 8.29 36.62 9.29 36.79 8.86 36.96 8.72 37.12 8.43 37.29 8.58 37.46 8.72 37.62 8.29 37.79 8.29 37.96 8.43 38.13 9.29 38.29 8.29 38.46 8.15 38.63 8.29 38.8 8.29 38.96 8.29 39.13 8.29 39.3 8.29 39.47 8.29 39.63 8.29 39.8 8.29 39.97 8.43 40.13 8.29 40.3 8.29 40.47 8.72 40.64 8.29 40.8 8.29 40.97 8.58 41.14 9.15 41.31 9.01 41.47 9.29 41.64 9.29 41.81 8.58 41.98 8.86 42.14 10.16 42.31 9.44 42.48 8.86 42.64 8.72 42.75 8.29 42.92 8.86 43.08 9.01 43.25 8.58 43.42 9.73 43.59 8.29 43.75 8.58 43.92 9.01 44.09 8.29 44.26 8.29 44.42 8.58 44.59 8.86 44.76 8.43 44.92 8.43 45.09 8.43 45.26 8.29 45.43 8.29 45.59 8.29 45.76 8.29 45.93 8.29 46.1 8.72 46.26 8.29 46.43 8.29 46.6 8.29 46.76 8.29 46.93 8.29 47.1 8.29 47.27 8.43 47.43 8.29 47.6 8.29 47.77 8.29 47.94 8.29 48.1 8.29 48.27 8.29 48.44 8.58 48.61 8.43 48.77 8.15 48.94 8.29 49.11 9.44 49.27 8.29 49.44 8.43 49.61 8.15 49.78 8.15 49.94 8.29 50.11 8.15 50.28 8.72 50.38 8.29 50.55 8.43 50.72 8.29 50.89 8.72 51.05 8.72 51.22 8.29 51.39 8.29 51.55 8.29 51.72 8.29 51.89 8.43 52.06 8.58 52.22 8.29 52.39 8.43 52.56 8.43 52.73 8.43 52.89 8.29 53.06 8.29 53.23 8.29 53.4 8.29 53.56 8.29 53.73 8.29 53.9 8.29 54.06 8.29 54.23 8.29 54.4 8.58 54.57 15.46 54.73 8.58 54.9 8.29 55.07 8.29 55.24 8.86 55.4 8.29 55.57 8.29 55.74 8.58 55.9 8.29 56.07 8.29 56.24 8.29 56.41 8.43 56.57 8.29 56.74 8.15 56.91 8.58 57.08 8.58 57.24 9.01 57.41 8.15 57.58 8.72 57.75 8.29 57.91 8.72 58.04 9.15 58.21 9.15 58.37 9.01 58.54 8.86 58.71 8.86 58.87 8.43 59.04 8.58 59.21 8.29 59.38 11.45 59.54 10.3 59.71 8.86 59.88 8.29 60.05 8.43 60.21 8.72 60.38 9.15 60.55 8.43 60.72 9.15 60.88 8.58 61.05 8.29 61.22 8.29 61.38 9.01 61.55 8.43 61.72 9.15 61.89 9.29 62.05 8.29 62.22 8.29 62.39 8.58 62.56 8.43 62.72 8.43 62.89 8.29 63.06 8.72 63.23 8.29 63.39 8.29 63.56 8.29 63.73 8.29 63.89 8.29 64.06 8.29 64.23 8.15 64.4 8.29 64.56 8.72 64.73 8.43 64.9 8.43 65.07 8.58 65.23 9.29 65.4 8.72 65.57 8.72 65.67 10.01 65.84 8.58 66.01 8.86 66.17 19.62 66.34 9.87 66.51 10.01 66.68 8.29 66.84 8 67.01 8 67.18 9.01 67.35 8.43 67.51 21.92 67.68 78.72 67.85 78.72 68.01 78.72 68.18 7.86 68.35 7.86 68.52 7.86 68.68 7.86 68.85 8.29 69.02 8.29 69.19 8.43 69.35 8.43 69.52 8.29 69.69 8.15 69.86 8.43 70.02 8.29 70.19 8.15 70.36 8.15 70.52 8.29 70.69 8.29 70.86 8.29 71.03 8.29 71.19 8.29 71.36 8.29 71.53 8.29 71.7 8.29 71.86 8.29 72.03 8.15 72.2 8.29 72.37 8.29 72.53 8.29 72.7 8.29 72.87 8.15 73.03 8.86 73.2 8.43 73.31 8.86 73.47 8.72 73.64 8.15 73.81 8.58 73.98 8.43 74.14 8.15 74.31 8.15 74.48 8.29 74.65 62.51 74.81 9.44 74.98 8.29 75.15 8.72 75.31 8.29 75.48 8.29 75.65 8.43 75.82 8.29 75.98 8.15 76.15 8.29 76.32 8.58 76.49 8.29 76.65 8.29 76.82 8.29 76.99 8.29 77.15 8.29 77.32 8.29 77.49 8.29 77.66 8.29 77.82 8.29 77.99 8.29 78.16 8.29 78.33 8.29 78.49 8.29 78.66 8.29 78.83 8.29 79 8.29 79.16 8.15 79.33 8.29 79.5 8.72 79.66 8.29 79.83 8.29 80 8.29 80.17 8.72 80.33 8.29 80.5 8.29 80.67 8.29 80.84 8.29 80.94 9.73 81.11 11.3 81.28 8.72 81.44 9.29 81.61 8.15 81.78 9.73 81.94 9.29 82.11 8.15 82.28 8.72 82.45 8.15 82.61 14.6 82.78 8.29 82.95 8.43 83.12 8.58 83.28 8.29 83.45 8.29 83.62 8.15 83.79 8.15 83.95 8.43 84.12 8.29 84.29 8.15 84.45 8.29 84.62 8.29 84.79 8.15 84.96 8.29 85.12 8.29 85.29 8.29 85.46 8.29 85.63 8.15 85.79 8.15 85.96 8.15 86.13 8.15 86.3 8.29 86.46 8.15 86.63 8.15 86.8 8.29 86.96 8.15 87.13 8.15 87.3 8.29 87.47 8.58 87.63 8.29 87.8 8.29 87.97 8.43 88.14 8.86 88.3 8.72 88.47 13.88 88.6 8.29 88.76 8.29 88.93 8.29 89.1 8.58 89.26 13.31 89.43 8.43 89.6 8.29 89.77 8.29 89.93 8.72 90.1 15.46 90.27 8.15 90.44 8.43 90.6 8.43 90.77 8.43 90.94 8.43 91.11 8.29 91.27 8.29 91.44 8.29 91.61 8.29 91.77 8.29 91.94 8.29 92.11 8.29 92.28 8.29 92.44 8.29 92.61 8.29 92.78 8.15 92.95 8.29 93.11 8.43 93.28 8.15 93.45 8.15 93.62 8.15 93.78 8.15 93.95 8.29 94.12 8.29 94.28 8.29 94.45 8.29 94.62 8.29 94.79 8.15 94.95 8.29 95.12 8.15 95.29 8.29 95.46 8.43 95.62 8.29 95.79 8.86 95.96 8.86 96.13 8.43 96.23 9.01 96.4 8.29 96.56 8.15 96.73 8.29 96.9 8.29 97.07 8.29 97.23 8.29 97.4 8.58 97.57 8.58 97.74 8.29 97.9 8.43 98.07 8.72 98.24 8.58 98.41 8.43 98.57 8.72 98.74 8.29 98.91 8.29 99.07 8.43 99.24 8.15 99.41 8.15 99.58 8.29 99.74 8.15 99.91 8.43 100.08 8.86 100.25 8.43 100.41 8.43 100.58 8.15 100.75 8.15 100.91 8.29 101.08 8.15 101.25 8.43 101.42 8.43 101.58 8.43 101.75 8.29 101.92 8.29 102.09 8.15 102.25 8.29 102.42 8.29 102.59 8.29 102.76 8.43 102.92 8.72 103.09 8.29 103.26 7.72 103.42 7.57 103.59 7.57 103.76 7.29"/>
      </g>
    </g>
    <g opacity="0" class="guide zoomslider" stroke="#000000" stroke-opacity="0.000" id="img-e512d6eb-11">
      <g fill="#EAEAEA" stroke-width="0.3" stroke-opacity="0" stroke="#6A6A6A" id="img-e512d6eb-12">
        <rect x="129.42" y="8" width="4" height="4"/>
        <g class="button_logo" fill="#6A6A6A" id="img-e512d6eb-13">
          <path d="M130.22,9.6 L 131.02 9.6 131.02 8.8 131.82 8.8 131.82 9.6 132.62 9.6 132.62 10.4 131.82 10.4 131.82 11.2 131.02 11.2 131.02 10.4 130.22 10.4 z"/>
        </g>
      </g>
      <g fill="#EAEAEA" id="img-e512d6eb-14">
        <rect x="109.92" y="8" width="19" height="4"/>
      </g>
      <g class="zoomslider_thumb" fill="#6A6A6A" id="img-e512d6eb-15">
        <rect x="118.42" y="8" width="2" height="4"/>
      </g>
      <g fill="#EAEAEA" stroke-width="0.3" stroke-opacity="0" stroke="#6A6A6A" id="img-e512d6eb-16">
        <rect x="105.42" y="8" width="4" height="4"/>
        <g class="button_logo" fill="#6A6A6A" id="img-e512d6eb-17">
          <path d="M106.22,9.6 L 108.62 9.6 108.62 10.4 106.22 10.4 z"/>
        </g>
      </g>
    </g>
  </g>
</g>
  <g class="guide ylabels" font-size="2.82" font-family="'PT Sans Caption','Helvetica Neue','Helvetica',sans-serif" fill="#6C606B" id="img-e512d6eb-18">
    <text x="16.83" y="164.77" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">-600</text>
    <text x="16.83" y="150.43" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">-500</text>
    <text x="16.83" y="136.09" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">-400</text>
    <text x="16.83" y="121.74" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">-300</text>
    <text x="16.83" y="107.4" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">-200</text>
    <text x="16.83" y="93.06" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">-100</text>
    <text x="16.83" y="78.72" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="visible">0</text>
    <text x="16.83" y="64.37" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="visible">100</text>
    <text x="16.83" y="50.03" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="visible">200</text>
    <text x="16.83" y="35.69" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="visible">300</text>
    <text x="16.83" y="21.34" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="visible">400</text>
    <text x="16.83" y="7" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="visible">500</text>
    <text x="16.83" y="-7.34" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">600</text>
    <text x="16.83" y="-21.69" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">700</text>
    <text x="16.83" y="-36.03" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">800</text>
    <text x="16.83" y="-50.37" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">900</text>
    <text x="16.83" y="-64.72" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">1000</text>
    <text x="16.83" y="-79.06" text-anchor="end" dy="0.35em" gadfly:scale="1.0" visibility="hidden">1100</text>
    <text x="16.83" y="150.43" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-500</text>
    <text x="16.83" y="147.56" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-480</text>
    <text x="16.83" y="144.69" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-460</text>
    <text x="16.83" y="141.82" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-440</text>
    <text x="16.83" y="138.96" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-420</text>
    <text x="16.83" y="136.09" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-400</text>
    <text x="16.83" y="133.22" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-380</text>
    <text x="16.83" y="130.35" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-360</text>
    <text x="16.83" y="127.48" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-340</text>
    <text x="16.83" y="124.61" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-320</text>
    <text x="16.83" y="121.74" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-300</text>
    <text x="16.83" y="118.88" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-280</text>
    <text x="16.83" y="116.01" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-260</text>
    <text x="16.83" y="113.14" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-240</text>
    <text x="16.83" y="110.27" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-220</text>
    <text x="16.83" y="107.4" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-200</text>
    <text x="16.83" y="104.53" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-180</text>
    <text x="16.83" y="101.66" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-160</text>
    <text x="16.83" y="98.8" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-140</text>
    <text x="16.83" y="95.93" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-120</text>
    <text x="16.83" y="93.06" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-100</text>
    <text x="16.83" y="90.19" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-80</text>
    <text x="16.83" y="87.32" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-60</text>
    <text x="16.83" y="84.45" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-40</text>
    <text x="16.83" y="81.58" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">-20</text>
    <text x="16.83" y="78.72" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">0</text>
    <text x="16.83" y="75.85" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">20</text>
    <text x="16.83" y="72.98" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">40</text>
    <text x="16.83" y="70.11" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">60</text>
    <text x="16.83" y="67.24" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">80</text>
    <text x="16.83" y="64.37" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">100</text>
    <text x="16.83" y="61.5" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">120</text>
    <text x="16.83" y="58.63" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">140</text>
    <text x="16.83" y="55.77" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">160</text>
    <text x="16.83" y="52.9" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">180</text>
    <text x="16.83" y="50.03" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">200</text>
    <text x="16.83" y="47.16" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">220</text>
    <text x="16.83" y="44.29" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">240</text>
    <text x="16.83" y="41.42" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">260</text>
    <text x="16.83" y="38.55" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">280</text>
    <text x="16.83" y="35.69" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">300</text>
    <text x="16.83" y="32.82" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">320</text>
    <text x="16.83" y="29.95" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">340</text>
    <text x="16.83" y="27.08" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">360</text>
    <text x="16.83" y="24.21" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">380</text>
    <text x="16.83" y="21.34" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">400</text>
    <text x="16.83" y="18.47" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">420</text>
    <text x="16.83" y="15.61" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">440</text>
    <text x="16.83" y="12.74" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">460</text>
    <text x="16.83" y="9.87" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">480</text>
    <text x="16.83" y="7" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">500</text>
    <text x="16.83" y="4.13" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">520</text>
    <text x="16.83" y="1.26" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">540</text>
    <text x="16.83" y="-1.61" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">560</text>
    <text x="16.83" y="-4.47" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">580</text>
    <text x="16.83" y="-7.34" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">600</text>
    <text x="16.83" y="-10.21" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">620</text>
    <text x="16.83" y="-13.08" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">640</text>
    <text x="16.83" y="-15.95" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">660</text>
    <text x="16.83" y="-18.82" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">680</text>
    <text x="16.83" y="-21.69" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">700</text>
    <text x="16.83" y="-24.55" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">720</text>
    <text x="16.83" y="-27.42" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">740</text>
    <text x="16.83" y="-30.29" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">760</text>
    <text x="16.83" y="-33.16" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">780</text>
    <text x="16.83" y="-36.03" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">800</text>
    <text x="16.83" y="-38.9" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">820</text>
    <text x="16.83" y="-41.77" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">840</text>
    <text x="16.83" y="-44.63" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">860</text>
    <text x="16.83" y="-47.5" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">880</text>
    <text x="16.83" y="-50.37" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">900</text>
    <text x="16.83" y="-53.24" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">920</text>
    <text x="16.83" y="-56.11" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">940</text>
    <text x="16.83" y="-58.98" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">960</text>
    <text x="16.83" y="-61.85" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">980</text>
    <text x="16.83" y="-64.72" text-anchor="end" dy="0.35em" gadfly:scale="10.0" visibility="hidden">1000</text>
    <text x="16.83" y="150.43" text-anchor="end" dy="0.35em" gadfly:scale="0.5" visibility="hidden">-500</text>
    <text x="16.83" y="78.72" text-anchor="end" dy="0.35em" gadfly:scale="0.5" visibility="hidden">0</text>
    <text x="16.83" y="7" text-anchor="end" dy="0.35em" gadfly:scale="0.5" visibility="hidden">500</text>
    <text x="16.83" y="-64.72" text-anchor="end" dy="0.35em" gadfly:scale="0.5" visibility="hidden">1000</text>
    <text x="16.83" y="150.43" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-500</text>
    <text x="16.83" y="143.26" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-450</text>
    <text x="16.83" y="136.09" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-400</text>
    <text x="16.83" y="128.92" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-350</text>
    <text x="16.83" y="121.74" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-300</text>
    <text x="16.83" y="114.57" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-250</text>
    <text x="16.83" y="107.4" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-200</text>
    <text x="16.83" y="100.23" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-150</text>
    <text x="16.83" y="93.06" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-100</text>
    <text x="16.83" y="85.89" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">-50</text>
    <text x="16.83" y="78.72" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">0</text>
    <text x="16.83" y="71.54" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">50</text>
    <text x="16.83" y="64.37" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">100</text>
    <text x="16.83" y="57.2" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">150</text>
    <text x="16.83" y="50.03" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">200</text>
    <text x="16.83" y="42.86" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">250</text>
    <text x="16.83" y="35.69" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">300</text>
    <text x="16.83" y="28.51" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">350</text>
    <text x="16.83" y="21.34" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">400</text>
    <text x="16.83" y="14.17" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">450</text>
    <text x="16.83" y="7" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">500</text>
    <text x="16.83" y="-0.17" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">550</text>
    <text x="16.83" y="-7.34" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">600</text>
    <text x="16.83" y="-14.51" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">650</text>
    <text x="16.83" y="-21.69" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">700</text>
    <text x="16.83" y="-28.86" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">750</text>
    <text x="16.83" y="-36.03" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">800</text>
    <text x="16.83" y="-43.2" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">850</text>
    <text x="16.83" y="-50.37" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">900</text>
    <text x="16.83" y="-57.54" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">950</text>
    <text x="16.83" y="-64.72" text-anchor="end" dy="0.35em" gadfly:scale="5.0" visibility="hidden">1000</text>
  </g>
  <g font-size="3.88" font-family="'PT Sans','Helvetica Neue','Helvetica',sans-serif" fill="#564A55" stroke="#000000" stroke-opacity="0.000" id="img-e512d6eb-19">
    <text x="8.81" y="42.86" text-anchor="end" dy="0.35em">y</text>
  </g>
</g>
<defs>
  <clipPath id="img-e512d6eb-4">
  <path d="M17.83,5 L 136.42 5 136.42 80.72 17.83 80.72" />
</clipPath>
</defs>
<script> <![CDATA[
(function(N){var k=/[\.\/]/,L=/\s*,\s*/,C=function(a,d){return a-d},a,v,y={n:{}},M=function(){for(var a=0,d=this.length;a<d;a++)if("undefined"!=typeof this[a])return this[a]},A=function(){for(var a=this.length;--a;)if("undefined"!=typeof this[a])return this[a]},w=function(k,d){k=String(k);var f=v,n=Array.prototype.slice.call(arguments,2),u=w.listeners(k),p=0,b,q=[],e={},l=[],r=a;l.firstDefined=M;l.lastDefined=A;a=k;for(var s=v=0,x=u.length;s<x;s++)"zIndex"in u[s]&&(q.push(u[s].zIndex),0>u[s].zIndex&&
(e[u[s].zIndex]=u[s]));for(q.sort(C);0>q[p];)if(b=e[q[p++] ],l.push(b.apply(d,n)),v)return v=f,l;for(s=0;s<x;s++)if(b=u[s],"zIndex"in b)if(b.zIndex==q[p]){l.push(b.apply(d,n));if(v)break;do if(p++,(b=e[q[p] ])&&l.push(b.apply(d,n)),v)break;while(b)}else e[b.zIndex]=b;else if(l.push(b.apply(d,n)),v)break;v=f;a=r;return l};w._events=y;w.listeners=function(a){a=a.split(k);var d=y,f,n,u,p,b,q,e,l=[d],r=[];u=0;for(p=a.length;u<p;u++){e=[];b=0;for(q=l.length;b<q;b++)for(d=l[b].n,f=[d[a[u] ],d["*"] ],n=2;n--;)if(d=
f[n])e.push(d),r=r.concat(d.f||[]);l=e}return r};w.on=function(a,d){a=String(a);if("function"!=typeof d)return function(){};for(var f=a.split(L),n=0,u=f.length;n<u;n++)(function(a){a=a.split(k);for(var b=y,f,e=0,l=a.length;e<l;e++)b=b.n,b=b.hasOwnProperty(a[e])&&b[a[e] ]||(b[a[e] ]={n:{}});b.f=b.f||[];e=0;for(l=b.f.length;e<l;e++)if(b.f[e]==d){f=!0;break}!f&&b.f.push(d)})(f[n]);return function(a){+a==+a&&(d.zIndex=+a)}};w.f=function(a){var d=[].slice.call(arguments,1);return function(){w.apply(null,
[a,null].concat(d).concat([].slice.call(arguments,0)))}};w.stop=function(){v=1};w.nt=function(k){return k?(new RegExp("(?:\\.|\\/|^)"+k+"(?:\\.|\\/|$)")).test(a):a};w.nts=function(){return a.split(k)};w.off=w.unbind=function(a,d){if(a){var f=a.split(L);if(1<f.length)for(var n=0,u=f.length;n<u;n++)w.off(f[n],d);else{for(var f=a.split(k),p,b,q,e,l=[y],n=0,u=f.length;n<u;n++)for(e=0;e<l.length;e+=q.length-2){q=[e,1];p=l[e].n;if("*"!=f[n])p[f[n] ]&&q.push(p[f[n] ]);else for(b in p)p.hasOwnProperty(b)&&
q.push(p[b]);l.splice.apply(l,q)}n=0;for(u=l.length;n<u;n++)for(p=l[n];p.n;){if(d){if(p.f){e=0;for(f=p.f.length;e<f;e++)if(p.f[e]==d){p.f.splice(e,1);break}!p.f.length&&delete p.f}for(b in p.n)if(p.n.hasOwnProperty(b)&&p.n[b].f){q=p.n[b].f;e=0;for(f=q.length;e<f;e++)if(q[e]==d){q.splice(e,1);break}!q.length&&delete p.n[b].f}}else for(b in delete p.f,p.n)p.n.hasOwnProperty(b)&&p.n[b].f&&delete p.n[b].f;p=p.n}}}else w._events=y={n:{}}};w.once=function(a,d){var f=function(){w.unbind(a,f);return d.apply(this,
arguments)};return w.on(a,f)};w.version="0.4.2";w.toString=function(){return"You are running Eve 0.4.2"};"undefined"!=typeof module&&module.exports?module.exports=w:"function"===typeof define&&define.amd?define("eve",[],function(){return w}):N.eve=w})(this);
(function(N,k){"function"===typeof define&&define.amd?define("Snap.svg",["eve"],function(L){return k(N,L)}):k(N,N.eve)})(this,function(N,k){var L=function(a){var k={},y=N.requestAnimationFrame||N.webkitRequestAnimationFrame||N.mozRequestAnimationFrame||N.oRequestAnimationFrame||N.msRequestAnimationFrame||function(a){setTimeout(a,16)},M=Array.isArray||function(a){return a instanceof Array||"[object Array]"==Object.prototype.toString.call(a)},A=0,w="M"+(+new Date).toString(36),z=function(a){if(null==
a)return this.s;var b=this.s-a;this.b+=this.dur*b;this.B+=this.dur*b;this.s=a},d=function(a){if(null==a)return this.spd;this.spd=a},f=function(a){if(null==a)return this.dur;this.s=this.s*a/this.dur;this.dur=a},n=function(){delete k[this.id];this.update();a("mina.stop."+this.id,this)},u=function(){this.pdif||(delete k[this.id],this.update(),this.pdif=this.get()-this.b)},p=function(){this.pdif&&(this.b=this.get()-this.pdif,delete this.pdif,k[this.id]=this)},b=function(){var a;if(M(this.start)){a=[];
for(var b=0,e=this.start.length;b<e;b++)a[b]=+this.start[b]+(this.end[b]-this.start[b])*this.easing(this.s)}else a=+this.start+(this.end-this.start)*this.easing(this.s);this.set(a)},q=function(){var l=0,b;for(b in k)if(k.hasOwnProperty(b)){var e=k[b],f=e.get();l++;e.s=(f-e.b)/(e.dur/e.spd);1<=e.s&&(delete k[b],e.s=1,l--,function(b){setTimeout(function(){a("mina.finish."+b.id,b)})}(e));e.update()}l&&y(q)},e=function(a,r,s,x,G,h,J){a={id:w+(A++).toString(36),start:a,end:r,b:s,s:0,dur:x-s,spd:1,get:G,
set:h,easing:J||e.linear,status:z,speed:d,duration:f,stop:n,pause:u,resume:p,update:b};k[a.id]=a;r=0;for(var K in k)if(k.hasOwnProperty(K)&&(r++,2==r))break;1==r&&y(q);return a};e.time=Date.now||function(){return+new Date};e.getById=function(a){return k[a]||null};e.linear=function(a){return a};e.easeout=function(a){return Math.pow(a,1.7)};e.easein=function(a){return Math.pow(a,0.48)};e.easeinout=function(a){if(1==a)return 1;if(0==a)return 0;var b=0.48-a/1.04,e=Math.sqrt(0.1734+b*b);a=e-b;a=Math.pow(Math.abs(a),
1/3)*(0>a?-1:1);b=-e-b;b=Math.pow(Math.abs(b),1/3)*(0>b?-1:1);a=a+b+0.5;return 3*(1-a)*a*a+a*a*a};e.backin=function(a){return 1==a?1:a*a*(2.70158*a-1.70158)};e.backout=function(a){if(0==a)return 0;a-=1;return a*a*(2.70158*a+1.70158)+1};e.elastic=function(a){return a==!!a?a:Math.pow(2,-10*a)*Math.sin(2*(a-0.075)*Math.PI/0.3)+1};e.bounce=function(a){a<1/2.75?a*=7.5625*a:a<2/2.75?(a-=1.5/2.75,a=7.5625*a*a+0.75):a<2.5/2.75?(a-=2.25/2.75,a=7.5625*a*a+0.9375):(a-=2.625/2.75,a=7.5625*a*a+0.984375);return a};
return N.mina=e}("undefined"==typeof k?function(){}:k),C=function(){function a(c,t){if(c){if(c.tagName)return x(c);if(y(c,"array")&&a.set)return a.set.apply(a,c);if(c instanceof e)return c;if(null==t)return c=G.doc.querySelector(c),x(c)}return new s(null==c?"100%":c,null==t?"100%":t)}function v(c,a){if(a){"#text"==c&&(c=G.doc.createTextNode(a.text||""));"string"==typeof c&&(c=v(c));if("string"==typeof a)return"xlink:"==a.substring(0,6)?c.getAttributeNS(m,a.substring(6)):"xml:"==a.substring(0,4)?c.getAttributeNS(la,
a.substring(4)):c.getAttribute(a);for(var da in a)if(a[h](da)){var b=J(a[da]);b?"xlink:"==da.substring(0,6)?c.setAttributeNS(m,da.substring(6),b):"xml:"==da.substring(0,4)?c.setAttributeNS(la,da.substring(4),b):c.setAttribute(da,b):c.removeAttribute(da)}}else c=G.doc.createElementNS(la,c);return c}function y(c,a){a=J.prototype.toLowerCase.call(a);return"finite"==a?isFinite(c):"array"==a&&(c instanceof Array||Array.isArray&&Array.isArray(c))?!0:"null"==a&&null===c||a==typeof c&&null!==c||"object"==
a&&c===Object(c)||$.call(c).slice(8,-1).toLowerCase()==a}function M(c){if("function"==typeof c||Object(c)!==c)return c;var a=new c.constructor,b;for(b in c)c[h](b)&&(a[b]=M(c[b]));return a}function A(c,a,b){function m(){var e=Array.prototype.slice.call(arguments,0),f=e.join("\u2400"),d=m.cache=m.cache||{},l=m.count=m.count||[];if(d[h](f)){a:for(var e=l,l=f,B=0,H=e.length;B<H;B++)if(e[B]===l){e.push(e.splice(B,1)[0]);break a}return b?b(d[f]):d[f]}1E3<=l.length&&delete d[l.shift()];l.push(f);d[f]=c.apply(a,
e);return b?b(d[f]):d[f]}return m}function w(c,a,b,m,e,f){return null==e?(c-=b,a-=m,c||a?(180*I.atan2(-a,-c)/C+540)%360:0):w(c,a,e,f)-w(b,m,e,f)}function z(c){return c%360*C/180}function d(c){var a=[];c=c.replace(/(?:^|\s)(\w+)\(([^)]+)\)/g,function(c,b,m){m=m.split(/\s*,\s*|\s+/);"rotate"==b&&1==m.length&&m.push(0,0);"scale"==b&&(2<m.length?m=m.slice(0,2):2==m.length&&m.push(0,0),1==m.length&&m.push(m[0],0,0));"skewX"==b?a.push(["m",1,0,I.tan(z(m[0])),1,0,0]):"skewY"==b?a.push(["m",1,I.tan(z(m[0])),
0,1,0,0]):a.push([b.charAt(0)].concat(m));return c});return a}function f(c,t){var b=O(c),m=new a.Matrix;if(b)for(var e=0,f=b.length;e<f;e++){var h=b[e],d=h.length,B=J(h[0]).toLowerCase(),H=h[0]!=B,l=H?m.invert():0,E;"t"==B&&2==d?m.translate(h[1],0):"t"==B&&3==d?H?(d=l.x(0,0),B=l.y(0,0),H=l.x(h[1],h[2]),l=l.y(h[1],h[2]),m.translate(H-d,l-B)):m.translate(h[1],h[2]):"r"==B?2==d?(E=E||t,m.rotate(h[1],E.x+E.width/2,E.y+E.height/2)):4==d&&(H?(H=l.x(h[2],h[3]),l=l.y(h[2],h[3]),m.rotate(h[1],H,l)):m.rotate(h[1],
h[2],h[3])):"s"==B?2==d||3==d?(E=E||t,m.scale(h[1],h[d-1],E.x+E.width/2,E.y+E.height/2)):4==d?H?(H=l.x(h[2],h[3]),l=l.y(h[2],h[3]),m.scale(h[1],h[1],H,l)):m.scale(h[1],h[1],h[2],h[3]):5==d&&(H?(H=l.x(h[3],h[4]),l=l.y(h[3],h[4]),m.scale(h[1],h[2],H,l)):m.scale(h[1],h[2],h[3],h[4])):"m"==B&&7==d&&m.add(h[1],h[2],h[3],h[4],h[5],h[6])}return m}function n(c,t){if(null==t){var m=!0;t="linearGradient"==c.type||"radialGradient"==c.type?c.node.getAttribute("gradientTransform"):"pattern"==c.type?c.node.getAttribute("patternTransform"):
c.node.getAttribute("transform");if(!t)return new a.Matrix;t=d(t)}else t=a._.rgTransform.test(t)?J(t).replace(/\.{3}|\u2026/g,c._.transform||aa):d(t),y(t,"array")&&(t=a.path?a.path.toString.call(t):J(t)),c._.transform=t;var b=f(t,c.getBBox(1));if(m)return b;c.matrix=b}function u(c){c=c.node.ownerSVGElement&&x(c.node.ownerSVGElement)||c.node.parentNode&&x(c.node.parentNode)||a.select("svg")||a(0,0);var t=c.select("defs"),t=null==t?!1:t.node;t||(t=r("defs",c.node).node);return t}function p(c){return c.node.ownerSVGElement&&
x(c.node.ownerSVGElement)||a.select("svg")}function b(c,a,m){function b(c){if(null==c)return aa;if(c==+c)return c;v(B,{width:c});try{return B.getBBox().width}catch(a){return 0}}function h(c){if(null==c)return aa;if(c==+c)return c;v(B,{height:c});try{return B.getBBox().height}catch(a){return 0}}function e(b,B){null==a?d[b]=B(c.attr(b)||0):b==a&&(d=B(null==m?c.attr(b)||0:m))}var f=p(c).node,d={},B=f.querySelector(".svg---mgr");B||(B=v("rect"),v(B,{x:-9E9,y:-9E9,width:10,height:10,"class":"svg---mgr",
fill:"none"}),f.appendChild(B));switch(c.type){case "rect":e("rx",b),e("ry",h);case "image":e("width",b),e("height",h);case "text":e("x",b);e("y",h);break;case "circle":e("cx",b);e("cy",h);e("r",b);break;case "ellipse":e("cx",b);e("cy",h);e("rx",b);e("ry",h);break;case "line":e("x1",b);e("x2",b);e("y1",h);e("y2",h);break;case "marker":e("refX",b);e("markerWidth",b);e("refY",h);e("markerHeight",h);break;case "radialGradient":e("fx",b);e("fy",h);break;case "tspan":e("dx",b);e("dy",h);break;default:e(a,
b)}f.removeChild(B);return d}function q(c){y(c,"array")||(c=Array.prototype.slice.call(arguments,0));for(var a=0,b=0,m=this.node;this[a];)delete this[a++];for(a=0;a<c.length;a++)"set"==c[a].type?c[a].forEach(function(c){m.appendChild(c.node)}):m.appendChild(c[a].node);for(var h=m.childNodes,a=0;a<h.length;a++)this[b++]=x(h[a]);return this}function e(c){if(c.snap in E)return E[c.snap];var a=this.id=V(),b;try{b=c.ownerSVGElement}catch(m){}this.node=c;b&&(this.paper=new s(b));this.type=c.tagName;this.anims=
{};this._={transform:[]};c.snap=a;E[a]=this;"g"==this.type&&(this.add=q);if(this.type in{g:1,mask:1,pattern:1})for(var e in s.prototype)s.prototype[h](e)&&(this[e]=s.prototype[e])}function l(c){this.node=c}function r(c,a){var b=v(c);a.appendChild(b);return x(b)}function s(c,a){var b,m,f,d=s.prototype;if(c&&"svg"==c.tagName){if(c.snap in E)return E[c.snap];var l=c.ownerDocument;b=new e(c);m=c.getElementsByTagName("desc")[0];f=c.getElementsByTagName("defs")[0];m||(m=v("desc"),m.appendChild(l.createTextNode("Created with Snap")),
b.node.appendChild(m));f||(f=v("defs"),b.node.appendChild(f));b.defs=f;for(var ca in d)d[h](ca)&&(b[ca]=d[ca]);b.paper=b.root=b}else b=r("svg",G.doc.body),v(b.node,{height:a,version:1.1,width:c,xmlns:la});return b}function x(c){return!c||c instanceof e||c instanceof l?c:c.tagName&&"svg"==c.tagName.toLowerCase()?new s(c):c.tagName&&"object"==c.tagName.toLowerCase()&&"image/svg+xml"==c.type?new s(c.contentDocument.getElementsByTagName("svg")[0]):new e(c)}a.version="0.3.0";a.toString=function(){return"Snap v"+
this.version};a._={};var G={win:N,doc:N.document};a._.glob=G;var h="hasOwnProperty",J=String,K=parseFloat,U=parseInt,I=Math,P=I.max,Q=I.min,Y=I.abs,C=I.PI,aa="",$=Object.prototype.toString,F=/^\s*((#[a-f\d]{6})|(#[a-f\d]{3})|rgba?\(\s*([\d\.]+%?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+%?(?:\s*,\s*[\d\.]+%?)?)\s*\)|hsba?\(\s*([\d\.]+(?:deg|\xb0|%)?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+(?:%?\s*,\s*[\d\.]+)?%?)\s*\)|hsla?\(\s*([\d\.]+(?:deg|\xb0|%)?\s*,\s*[\d\.]+%?\s*,\s*[\d\.]+(?:%?\s*,\s*[\d\.]+)?%?)\s*\))\s*$/i;a._.separator=
RegExp("[,\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]+");var S=RegExp("[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*"),X={hs:1,rg:1},W=RegExp("([a-z])[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029,]*((-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*)+)",
"ig"),ma=RegExp("([rstm])[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029,]*((-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*)+)","ig"),Z=RegExp("(-?\\d*\\.?\\d*(?:e[\\-+]?\\d+)?)[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*,?[\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*",
"ig"),na=0,ba="S"+(+new Date).toString(36),V=function(){return ba+(na++).toString(36)},m="http://www.w3.org/1999/xlink",la="http://www.w3.org/2000/svg",E={},ca=a.url=function(c){return"url('#"+c+"')"};a._.$=v;a._.id=V;a.format=function(){var c=/\{([^\}]+)\}/g,a=/(?:(?:^|\.)(.+?)(?=\[|\.|$|\()|\[('|")(.+?)\2\])(\(\))?/g,b=function(c,b,m){var h=m;b.replace(a,function(c,a,b,m,t){a=a||m;h&&(a in h&&(h=h[a]),"function"==typeof h&&t&&(h=h()))});return h=(null==h||h==m?c:h)+""};return function(a,m){return J(a).replace(c,
function(c,a){return b(c,a,m)})}}();a._.clone=M;a._.cacher=A;a.rad=z;a.deg=function(c){return 180*c/C%360};a.angle=w;a.is=y;a.snapTo=function(c,a,b){b=y(b,"finite")?b:10;if(y(c,"array"))for(var m=c.length;m--;){if(Y(c[m]-a)<=b)return c[m]}else{c=+c;m=a%c;if(m<b)return a-m;if(m>c-b)return a-m+c}return a};a.getRGB=A(function(c){if(!c||(c=J(c)).indexOf("-")+1)return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka};if("none"==c)return{r:-1,g:-1,b:-1,hex:"none",toString:ka};!X[h](c.toLowerCase().substring(0,
2))&&"#"!=c.charAt()&&(c=T(c));if(!c)return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka};var b,m,e,f,d;if(c=c.match(F)){c[2]&&(e=U(c[2].substring(5),16),m=U(c[2].substring(3,5),16),b=U(c[2].substring(1,3),16));c[3]&&(e=U((d=c[3].charAt(3))+d,16),m=U((d=c[3].charAt(2))+d,16),b=U((d=c[3].charAt(1))+d,16));c[4]&&(d=c[4].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b*=2.55),m=K(d[1]),"%"==d[1].slice(-1)&&(m*=2.55),e=K(d[2]),"%"==d[2].slice(-1)&&(e*=2.55),"rgba"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),
d[3]&&"%"==d[3].slice(-1)&&(f/=100));if(c[5])return d=c[5].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b/=100),m=K(d[1]),"%"==d[1].slice(-1)&&(m/=100),e=K(d[2]),"%"==d[2].slice(-1)&&(e/=100),"deg"!=d[0].slice(-3)&&"\u00b0"!=d[0].slice(-1)||(b/=360),"hsba"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),d[3]&&"%"==d[3].slice(-1)&&(f/=100),a.hsb2rgb(b,m,e,f);if(c[6])return d=c[6].split(S),b=K(d[0]),"%"==d[0].slice(-1)&&(b/=100),m=K(d[1]),"%"==d[1].slice(-1)&&(m/=100),e=K(d[2]),"%"==d[2].slice(-1)&&(e/=100),
"deg"!=d[0].slice(-3)&&"\u00b0"!=d[0].slice(-1)||(b/=360),"hsla"==c[1].toLowerCase().slice(0,4)&&(f=K(d[3])),d[3]&&"%"==d[3].slice(-1)&&(f/=100),a.hsl2rgb(b,m,e,f);b=Q(I.round(b),255);m=Q(I.round(m),255);e=Q(I.round(e),255);f=Q(P(f,0),1);c={r:b,g:m,b:e,toString:ka};c.hex="#"+(16777216|e|m<<8|b<<16).toString(16).slice(1);c.opacity=y(f,"finite")?f:1;return c}return{r:-1,g:-1,b:-1,hex:"none",error:1,toString:ka}},a);a.hsb=A(function(c,b,m){return a.hsb2rgb(c,b,m).hex});a.hsl=A(function(c,b,m){return a.hsl2rgb(c,
b,m).hex});a.rgb=A(function(c,a,b,m){if(y(m,"finite")){var e=I.round;return"rgba("+[e(c),e(a),e(b),+m.toFixed(2)]+")"}return"#"+(16777216|b|a<<8|c<<16).toString(16).slice(1)});var T=function(c){var a=G.doc.getElementsByTagName("head")[0]||G.doc.getElementsByTagName("svg")[0];T=A(function(c){if("red"==c.toLowerCase())return"rgb(255, 0, 0)";a.style.color="rgb(255, 0, 0)";a.style.color=c;c=G.doc.defaultView.getComputedStyle(a,aa).getPropertyValue("color");return"rgb(255, 0, 0)"==c?null:c});return T(c)},
qa=function(){return"hsb("+[this.h,this.s,this.b]+")"},ra=function(){return"hsl("+[this.h,this.s,this.l]+")"},ka=function(){return 1==this.opacity||null==this.opacity?this.hex:"rgba("+[this.r,this.g,this.b,this.opacity]+")"},D=function(c,b,m){null==b&&y(c,"object")&&"r"in c&&"g"in c&&"b"in c&&(m=c.b,b=c.g,c=c.r);null==b&&y(c,string)&&(m=a.getRGB(c),c=m.r,b=m.g,m=m.b);if(1<c||1<b||1<m)c/=255,b/=255,m/=255;return[c,b,m]},oa=function(c,b,m,e){c=I.round(255*c);b=I.round(255*b);m=I.round(255*m);c={r:c,
g:b,b:m,opacity:y(e,"finite")?e:1,hex:a.rgb(c,b,m),toString:ka};y(e,"finite")&&(c.opacity=e);return c};a.color=function(c){var b;y(c,"object")&&"h"in c&&"s"in c&&"b"in c?(b=a.hsb2rgb(c),c.r=b.r,c.g=b.g,c.b=b.b,c.opacity=1,c.hex=b.hex):y(c,"object")&&"h"in c&&"s"in c&&"l"in c?(b=a.hsl2rgb(c),c.r=b.r,c.g=b.g,c.b=b.b,c.opacity=1,c.hex=b.hex):(y(c,"string")&&(c=a.getRGB(c)),y(c,"object")&&"r"in c&&"g"in c&&"b"in c&&!("error"in c)?(b=a.rgb2hsl(c),c.h=b.h,c.s=b.s,c.l=b.l,b=a.rgb2hsb(c),c.v=b.b):(c={hex:"none"},
c.r=c.g=c.b=c.h=c.s=c.v=c.l=-1,c.error=1));c.toString=ka;return c};a.hsb2rgb=function(c,a,b,m){y(c,"object")&&"h"in c&&"s"in c&&"b"in c&&(b=c.b,a=c.s,c=c.h,m=c.o);var e,h,d;c=360*c%360/60;d=b*a;a=d*(1-Y(c%2-1));b=e=h=b-d;c=~~c;b+=[d,a,0,0,a,d][c];e+=[a,d,d,a,0,0][c];h+=[0,0,a,d,d,a][c];return oa(b,e,h,m)};a.hsl2rgb=function(c,a,b,m){y(c,"object")&&"h"in c&&"s"in c&&"l"in c&&(b=c.l,a=c.s,c=c.h);if(1<c||1<a||1<b)c/=360,a/=100,b/=100;var e,h,d;c=360*c%360/60;d=2*a*(0.5>b?b:1-b);a=d*(1-Y(c%2-1));b=e=
h=b-d/2;c=~~c;b+=[d,a,0,0,a,d][c];e+=[a,d,d,a,0,0][c];h+=[0,0,a,d,d,a][c];return oa(b,e,h,m)};a.rgb2hsb=function(c,a,b){b=D(c,a,b);c=b[0];a=b[1];b=b[2];var m,e;m=P(c,a,b);e=m-Q(c,a,b);c=((0==e?0:m==c?(a-b)/e:m==a?(b-c)/e+2:(c-a)/e+4)+360)%6*60/360;return{h:c,s:0==e?0:e/m,b:m,toString:qa}};a.rgb2hsl=function(c,a,b){b=D(c,a,b);c=b[0];a=b[1];b=b[2];var m,e,h;m=P(c,a,b);e=Q(c,a,b);h=m-e;c=((0==h?0:m==c?(a-b)/h:m==a?(b-c)/h+2:(c-a)/h+4)+360)%6*60/360;m=(m+e)/2;return{h:c,s:0==h?0:0.5>m?h/(2*m):h/(2-2*
m),l:m,toString:ra}};a.parsePathString=function(c){if(!c)return null;var b=a.path(c);if(b.arr)return a.path.clone(b.arr);var m={a:7,c:6,o:2,h:1,l:2,m:2,r:4,q:4,s:4,t:2,v:1,u:3,z:0},e=[];y(c,"array")&&y(c[0],"array")&&(e=a.path.clone(c));e.length||J(c).replace(W,function(c,a,b){var h=[];c=a.toLowerCase();b.replace(Z,function(c,a){a&&h.push(+a)});"m"==c&&2<h.length&&(e.push([a].concat(h.splice(0,2))),c="l",a="m"==a?"l":"L");"o"==c&&1==h.length&&e.push([a,h[0] ]);if("r"==c)e.push([a].concat(h));else for(;h.length>=
m[c]&&(e.push([a].concat(h.splice(0,m[c]))),m[c]););});e.toString=a.path.toString;b.arr=a.path.clone(e);return e};var O=a.parseTransformString=function(c){if(!c)return null;var b=[];y(c,"array")&&y(c[0],"array")&&(b=a.path.clone(c));b.length||J(c).replace(ma,function(c,a,m){var e=[];a.toLowerCase();m.replace(Z,function(c,a){a&&e.push(+a)});b.push([a].concat(e))});b.toString=a.path.toString;return b};a._.svgTransform2string=d;a._.rgTransform=RegExp("^[a-z][\t\n\x0B\f\r \u00a0\u1680\u180e\u2000\u2001\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200a\u202f\u205f\u3000\u2028\u2029]*-?\\.?\\d",
"i");a._.transform2matrix=f;a._unit2px=b;a._.getSomeDefs=u;a._.getSomeSVG=p;a.select=function(c){return x(G.doc.querySelector(c))};a.selectAll=function(c){c=G.doc.querySelectorAll(c);for(var b=(a.set||Array)(),m=0;m<c.length;m++)b.push(x(c[m]));return b};setInterval(function(){for(var c in E)if(E[h](c)){var a=E[c],b=a.node;("svg"!=a.type&&!b.ownerSVGElement||"svg"==a.type&&(!b.parentNode||"ownerSVGElement"in b.parentNode&&!b.ownerSVGElement))&&delete E[c]}},1E4);(function(c){function m(c){function a(c,
b){var m=v(c.node,b);(m=(m=m&&m.match(d))&&m[2])&&"#"==m.charAt()&&(m=m.substring(1))&&(f[m]=(f[m]||[]).concat(function(a){var m={};m[b]=ca(a);v(c.node,m)}))}function b(c){var a=v(c.node,"xlink:href");a&&"#"==a.charAt()&&(a=a.substring(1))&&(f[a]=(f[a]||[]).concat(function(a){c.attr("xlink:href","#"+a)}))}var e=c.selectAll("*"),h,d=/^\s*url\(("|'|)(.*)\1\)\s*$/;c=[];for(var f={},l=0,E=e.length;l<E;l++){h=e[l];a(h,"fill");a(h,"stroke");a(h,"filter");a(h,"mask");a(h,"clip-path");b(h);var t=v(h.node,
"id");t&&(v(h.node,{id:h.id}),c.push({old:t,id:h.id}))}l=0;for(E=c.length;l<E;l++)if(e=f[c[l].old])for(h=0,t=e.length;h<t;h++)e[h](c[l].id)}function e(c,a,b){return function(m){m=m.slice(c,a);1==m.length&&(m=m[0]);return b?b(m):m}}function d(c){return function(){var a=c?"<"+this.type:"",b=this.node.attributes,m=this.node.childNodes;if(c)for(var e=0,h=b.length;e<h;e++)a+=" "+b[e].name+'="'+b[e].value.replace(/"/g,'\\"')+'"';if(m.length){c&&(a+=">");e=0;for(h=m.length;e<h;e++)3==m[e].nodeType?a+=m[e].nodeValue:
1==m[e].nodeType&&(a+=x(m[e]).toString());c&&(a+="</"+this.type+">")}else c&&(a+="/>");return a}}c.attr=function(c,a){if(!c)return this;if(y(c,"string"))if(1<arguments.length){var b={};b[c]=a;c=b}else return k("snap.util.getattr."+c,this).firstDefined();for(var m in c)c[h](m)&&k("snap.util.attr."+m,this,c[m]);return this};c.getBBox=function(c){if(!a.Matrix||!a.path)return this.node.getBBox();var b=this,m=new a.Matrix;if(b.removed)return a._.box();for(;"use"==b.type;)if(c||(m=m.add(b.transform().localMatrix.translate(b.attr("x")||
0,b.attr("y")||0))),b.original)b=b.original;else var e=b.attr("xlink:href"),b=b.original=b.node.ownerDocument.getElementById(e.substring(e.indexOf("#")+1));var e=b._,h=a.path.get[b.type]||a.path.get.deflt;try{if(c)return e.bboxwt=h?a.path.getBBox(b.realPath=h(b)):a._.box(b.node.getBBox()),a._.box(e.bboxwt);b.realPath=h(b);b.matrix=b.transform().localMatrix;e.bbox=a.path.getBBox(a.path.map(b.realPath,m.add(b.matrix)));return a._.box(e.bbox)}catch(d){return a._.box()}};var f=function(){return this.string};
c.transform=function(c){var b=this._;if(null==c){var m=this;c=new a.Matrix(this.node.getCTM());for(var e=n(this),h=[e],d=new a.Matrix,l=e.toTransformString(),b=J(e)==J(this.matrix)?J(b.transform):l;"svg"!=m.type&&(m=m.parent());)h.push(n(m));for(m=h.length;m--;)d.add(h[m]);return{string:b,globalMatrix:c,totalMatrix:d,localMatrix:e,diffMatrix:c.clone().add(e.invert()),global:c.toTransformString(),total:d.toTransformString(),local:l,toString:f}}c instanceof a.Matrix?this.matrix=c:n(this,c);this.node&&
("linearGradient"==this.type||"radialGradient"==this.type?v(this.node,{gradientTransform:this.matrix}):"pattern"==this.type?v(this.node,{patternTransform:this.matrix}):v(this.node,{transform:this.matrix}));return this};c.parent=function(){return x(this.node.parentNode)};c.append=c.add=function(c){if(c){if("set"==c.type){var a=this;c.forEach(function(c){a.add(c)});return this}c=x(c);this.node.appendChild(c.node);c.paper=this.paper}return this};c.appendTo=function(c){c&&(c=x(c),c.append(this));return this};
c.prepend=function(c){if(c){if("set"==c.type){var a=this,b;c.forEach(function(c){b?b.after(c):a.prepend(c);b=c});return this}c=x(c);var m=c.parent();this.node.insertBefore(c.node,this.node.firstChild);this.add&&this.add();c.paper=this.paper;this.parent()&&this.parent().add();m&&m.add()}return this};c.prependTo=function(c){c=x(c);c.prepend(this);return this};c.before=function(c){if("set"==c.type){var a=this;c.forEach(function(c){var b=c.parent();a.node.parentNode.insertBefore(c.node,a.node);b&&b.add()});
this.parent().add();return this}c=x(c);var b=c.parent();this.node.parentNode.insertBefore(c.node,this.node);this.parent()&&this.parent().add();b&&b.add();c.paper=this.paper;return this};c.after=function(c){c=x(c);var a=c.parent();this.node.nextSibling?this.node.parentNode.insertBefore(c.node,this.node.nextSibling):this.node.parentNode.appendChild(c.node);this.parent()&&this.parent().add();a&&a.add();c.paper=this.paper;return this};c.insertBefore=function(c){c=x(c);var a=this.parent();c.node.parentNode.insertBefore(this.node,
c.node);this.paper=c.paper;a&&a.add();c.parent()&&c.parent().add();return this};c.insertAfter=function(c){c=x(c);var a=this.parent();c.node.parentNode.insertBefore(this.node,c.node.nextSibling);this.paper=c.paper;a&&a.add();c.parent()&&c.parent().add();return this};c.remove=function(){var c=this.parent();this.node.parentNode&&this.node.parentNode.removeChild(this.node);delete this.paper;this.removed=!0;c&&c.add();return this};c.select=function(c){return x(this.node.querySelector(c))};c.selectAll=
function(c){c=this.node.querySelectorAll(c);for(var b=(a.set||Array)(),m=0;m<c.length;m++)b.push(x(c[m]));return b};c.asPX=function(c,a){null==a&&(a=this.attr(c));return+b(this,c,a)};c.use=function(){var c,a=this.node.id;a||(a=this.id,v(this.node,{id:a}));c="linearGradient"==this.type||"radialGradient"==this.type||"pattern"==this.type?r(this.type,this.node.parentNode):r("use",this.node.parentNode);v(c.node,{"xlink:href":"#"+a});c.original=this;return c};var l=/\S+/g;c.addClass=function(c){var a=(c||
"").match(l)||[];c=this.node;var b=c.className.baseVal,m=b.match(l)||[],e,h,d;if(a.length){for(e=0;d=a[e++];)h=m.indexOf(d),~h||m.push(d);a=m.join(" ");b!=a&&(c.className.baseVal=a)}return this};c.removeClass=function(c){var a=(c||"").match(l)||[];c=this.node;var b=c.className.baseVal,m=b.match(l)||[],e,h;if(m.length){for(e=0;h=a[e++];)h=m.indexOf(h),~h&&m.splice(h,1);a=m.join(" ");b!=a&&(c.className.baseVal=a)}return this};c.hasClass=function(c){return!!~(this.node.className.baseVal.match(l)||[]).indexOf(c)};
c.toggleClass=function(c,a){if(null!=a)return a?this.addClass(c):this.removeClass(c);var b=(c||"").match(l)||[],m=this.node,e=m.className.baseVal,h=e.match(l)||[],d,f,E;for(d=0;E=b[d++];)f=h.indexOf(E),~f?h.splice(f,1):h.push(E);b=h.join(" ");e!=b&&(m.className.baseVal=b);return this};c.clone=function(){var c=x(this.node.cloneNode(!0));v(c.node,"id")&&v(c.node,{id:c.id});m(c);c.insertAfter(this);return c};c.toDefs=function(){u(this).appendChild(this.node);return this};c.pattern=c.toPattern=function(c,
a,b,m){var e=r("pattern",u(this));null==c&&(c=this.getBBox());y(c,"object")&&"x"in c&&(a=c.y,b=c.width,m=c.height,c=c.x);v(e.node,{x:c,y:a,width:b,height:m,patternUnits:"userSpaceOnUse",id:e.id,viewBox:[c,a,b,m].join(" ")});e.node.appendChild(this.node);return e};c.marker=function(c,a,b,m,e,h){var d=r("marker",u(this));null==c&&(c=this.getBBox());y(c,"object")&&"x"in c&&(a=c.y,b=c.width,m=c.height,e=c.refX||c.cx,h=c.refY||c.cy,c=c.x);v(d.node,{viewBox:[c,a,b,m].join(" "),markerWidth:b,markerHeight:m,
orient:"auto",refX:e||0,refY:h||0,id:d.id});d.node.appendChild(this.node);return d};var E=function(c,a,b,m){"function"!=typeof b||b.length||(m=b,b=L.linear);this.attr=c;this.dur=a;b&&(this.easing=b);m&&(this.callback=m)};a._.Animation=E;a.animation=function(c,a,b,m){return new E(c,a,b,m)};c.inAnim=function(){var c=[],a;for(a in this.anims)this.anims[h](a)&&function(a){c.push({anim:new E(a._attrs,a.dur,a.easing,a._callback),mina:a,curStatus:a.status(),status:function(c){return a.status(c)},stop:function(){a.stop()}})}(this.anims[a]);
return c};a.animate=function(c,a,b,m,e,h){"function"!=typeof e||e.length||(h=e,e=L.linear);var d=L.time();c=L(c,a,d,d+m,L.time,b,e);h&&k.once("mina.finish."+c.id,h);return c};c.stop=function(){for(var c=this.inAnim(),a=0,b=c.length;a<b;a++)c[a].stop();return this};c.animate=function(c,a,b,m){"function"!=typeof b||b.length||(m=b,b=L.linear);c instanceof E&&(m=c.callback,b=c.easing,a=b.dur,c=c.attr);var d=[],f=[],l={},t,ca,n,T=this,q;for(q in c)if(c[h](q)){T.equal?(n=T.equal(q,J(c[q])),t=n.from,ca=
n.to,n=n.f):(t=+T.attr(q),ca=+c[q]);var la=y(t,"array")?t.length:1;l[q]=e(d.length,d.length+la,n);d=d.concat(t);f=f.concat(ca)}t=L.time();var p=L(d,f,t,t+a,L.time,function(c){var a={},b;for(b in l)l[h](b)&&(a[b]=l[b](c));T.attr(a)},b);T.anims[p.id]=p;p._attrs=c;p._callback=m;k("snap.animcreated."+T.id,p);k.once("mina.finish."+p.id,function(){delete T.anims[p.id];m&&m.call(T)});k.once("mina.stop."+p.id,function(){delete T.anims[p.id]});return T};var T={};c.data=function(c,b){var m=T[this.id]=T[this.id]||
{};if(0==arguments.length)return k("snap.data.get."+this.id,this,m,null),m;if(1==arguments.length){if(a.is(c,"object")){for(var e in c)c[h](e)&&this.data(e,c[e]);return this}k("snap.data.get."+this.id,this,m[c],c);return m[c]}m[c]=b;k("snap.data.set."+this.id,this,b,c);return this};c.removeData=function(c){null==c?T[this.id]={}:T[this.id]&&delete T[this.id][c];return this};c.outerSVG=c.toString=d(1);c.innerSVG=d()})(e.prototype);a.parse=function(c){var a=G.doc.createDocumentFragment(),b=!0,m=G.doc.createElement("div");
c=J(c);c.match(/^\s*<\s*svg(?:\s|>)/)||(c="<svg>"+c+"</svg>",b=!1);m.innerHTML=c;if(c=m.getElementsByTagName("svg")[0])if(b)a=c;else for(;c.firstChild;)a.appendChild(c.firstChild);m.innerHTML=aa;return new l(a)};l.prototype.select=e.prototype.select;l.prototype.selectAll=e.prototype.selectAll;a.fragment=function(){for(var c=Array.prototype.slice.call(arguments,0),b=G.doc.createDocumentFragment(),m=0,e=c.length;m<e;m++){var h=c[m];h.node&&h.node.nodeType&&b.appendChild(h.node);h.nodeType&&b.appendChild(h);
"string"==typeof h&&b.appendChild(a.parse(h).node)}return new l(b)};a._.make=r;a._.wrap=x;s.prototype.el=function(c,a){var b=r(c,this.node);a&&b.attr(a);return b};k.on("snap.util.getattr",function(){var c=k.nt(),c=c.substring(c.lastIndexOf(".")+1),a=c.replace(/[A-Z]/g,function(c){return"-"+c.toLowerCase()});return pa[h](a)?this.node.ownerDocument.defaultView.getComputedStyle(this.node,null).getPropertyValue(a):v(this.node,c)});var pa={"alignment-baseline":0,"baseline-shift":0,clip:0,"clip-path":0,
"clip-rule":0,color:0,"color-interpolation":0,"color-interpolation-filters":0,"color-profile":0,"color-rendering":0,cursor:0,direction:0,display:0,"dominant-baseline":0,"enable-background":0,fill:0,"fill-opacity":0,"fill-rule":0,filter:0,"flood-color":0,"flood-opacity":0,font:0,"font-family":0,"font-size":0,"font-size-adjust":0,"font-stretch":0,"font-style":0,"font-variant":0,"font-weight":0,"glyph-orientation-horizontal":0,"glyph-orientation-vertical":0,"image-rendering":0,kerning:0,"letter-spacing":0,
"lighting-color":0,marker:0,"marker-end":0,"marker-mid":0,"marker-start":0,mask:0,opacity:0,overflow:0,"pointer-events":0,"shape-rendering":0,"stop-color":0,"stop-opacity":0,stroke:0,"stroke-dasharray":0,"stroke-dashoffset":0,"stroke-linecap":0,"stroke-linejoin":0,"stroke-miterlimit":0,"stroke-opacity":0,"stroke-width":0,"text-anchor":0,"text-decoration":0,"text-rendering":0,"unicode-bidi":0,visibility:0,"word-spacing":0,"writing-mode":0};k.on("snap.util.attr",function(c){var a=k.nt(),b={},a=a.substring(a.lastIndexOf(".")+
1);b[a]=c;var m=a.replace(/-(\w)/gi,function(c,a){return a.toUpperCase()}),a=a.replace(/[A-Z]/g,function(c){return"-"+c.toLowerCase()});pa[h](a)?this.node.style[m]=null==c?aa:c:v(this.node,b)});a.ajax=function(c,a,b,m){var e=new XMLHttpRequest,h=V();if(e){if(y(a,"function"))m=b,b=a,a=null;else if(y(a,"object")){var d=[],f;for(f in a)a.hasOwnProperty(f)&&d.push(encodeURIComponent(f)+"="+encodeURIComponent(a[f]));a=d.join("&")}e.open(a?"POST":"GET",c,!0);a&&(e.setRequestHeader("X-Requested-With","XMLHttpRequest"),
e.setRequestHeader("Content-type","application/x-www-form-urlencoded"));b&&(k.once("snap.ajax."+h+".0",b),k.once("snap.ajax."+h+".200",b),k.once("snap.ajax."+h+".304",b));e.onreadystatechange=function(){4==e.readyState&&k("snap.ajax."+h+"."+e.status,m,e)};if(4==e.readyState)return e;e.send(a);return e}};a.load=function(c,b,m){a.ajax(c,function(c){c=a.parse(c.responseText);m?b.call(m,c):b(c)})};a.getElementByPoint=function(c,a){var b,m,e=G.doc.elementFromPoint(c,a);if(G.win.opera&&"svg"==e.tagName){b=
e;m=b.getBoundingClientRect();b=b.ownerDocument;var h=b.body,d=b.documentElement;b=m.top+(g.win.pageYOffset||d.scrollTop||h.scrollTop)-(d.clientTop||h.clientTop||0);m=m.left+(g.win.pageXOffset||d.scrollLeft||h.scrollLeft)-(d.clientLeft||h.clientLeft||0);h=e.createSVGRect();h.x=c-m;h.y=a-b;h.width=h.height=1;b=e.getIntersectionList(h,null);b.length&&(e=b[b.length-1])}return e?x(e):null};a.plugin=function(c){c(a,e,s,G,l)};return G.win.Snap=a}();C.plugin(function(a,k,y,M,A){function w(a,d,f,b,q,e){null==
d&&"[object SVGMatrix]"==z.call(a)?(this.a=a.a,this.b=a.b,this.c=a.c,this.d=a.d,this.e=a.e,this.f=a.f):null!=a?(this.a=+a,this.b=+d,this.c=+f,this.d=+b,this.e=+q,this.f=+e):(this.a=1,this.c=this.b=0,this.d=1,this.f=this.e=0)}var z=Object.prototype.toString,d=String,f=Math;(function(n){function k(a){return a[0]*a[0]+a[1]*a[1]}function p(a){var d=f.sqrt(k(a));a[0]&&(a[0]/=d);a[1]&&(a[1]/=d)}n.add=function(a,d,e,f,n,p){var k=[[],[],[] ],u=[[this.a,this.c,this.e],[this.b,this.d,this.f],[0,0,1] ];d=[[a,
e,n],[d,f,p],[0,0,1] ];a&&a instanceof w&&(d=[[a.a,a.c,a.e],[a.b,a.d,a.f],[0,0,1] ]);for(a=0;3>a;a++)for(e=0;3>e;e++){for(f=n=0;3>f;f++)n+=u[a][f]*d[f][e];k[a][e]=n}this.a=k[0][0];this.b=k[1][0];this.c=k[0][1];this.d=k[1][1];this.e=k[0][2];this.f=k[1][2];return this};n.invert=function(){var a=this.a*this.d-this.b*this.c;return new w(this.d/a,-this.b/a,-this.c/a,this.a/a,(this.c*this.f-this.d*this.e)/a,(this.b*this.e-this.a*this.f)/a)};n.clone=function(){return new w(this.a,this.b,this.c,this.d,this.e,
this.f)};n.translate=function(a,d){return this.add(1,0,0,1,a,d)};n.scale=function(a,d,e,f){null==d&&(d=a);(e||f)&&this.add(1,0,0,1,e,f);this.add(a,0,0,d,0,0);(e||f)&&this.add(1,0,0,1,-e,-f);return this};n.rotate=function(b,d,e){b=a.rad(b);d=d||0;e=e||0;var l=+f.cos(b).toFixed(9);b=+f.sin(b).toFixed(9);this.add(l,b,-b,l,d,e);return this.add(1,0,0,1,-d,-e)};n.x=function(a,d){return a*this.a+d*this.c+this.e};n.y=function(a,d){return a*this.b+d*this.d+this.f};n.get=function(a){return+this[d.fromCharCode(97+
a)].toFixed(4)};n.toString=function(){return"matrix("+[this.get(0),this.get(1),this.get(2),this.get(3),this.get(4),this.get(5)].join()+")"};n.offset=function(){return[this.e.toFixed(4),this.f.toFixed(4)]};n.determinant=function(){return this.a*this.d-this.b*this.c};n.split=function(){var b={};b.dx=this.e;b.dy=this.f;var d=[[this.a,this.c],[this.b,this.d] ];b.scalex=f.sqrt(k(d[0]));p(d[0]);b.shear=d[0][0]*d[1][0]+d[0][1]*d[1][1];d[1]=[d[1][0]-d[0][0]*b.shear,d[1][1]-d[0][1]*b.shear];b.scaley=f.sqrt(k(d[1]));
p(d[1]);b.shear/=b.scaley;0>this.determinant()&&(b.scalex=-b.scalex);var e=-d[0][1],d=d[1][1];0>d?(b.rotate=a.deg(f.acos(d)),0>e&&(b.rotate=360-b.rotate)):b.rotate=a.deg(f.asin(e));b.isSimple=!+b.shear.toFixed(9)&&(b.scalex.toFixed(9)==b.scaley.toFixed(9)||!b.rotate);b.isSuperSimple=!+b.shear.toFixed(9)&&b.scalex.toFixed(9)==b.scaley.toFixed(9)&&!b.rotate;b.noRotation=!+b.shear.toFixed(9)&&!b.rotate;return b};n.toTransformString=function(a){a=a||this.split();if(+a.shear.toFixed(9))return"m"+[this.get(0),
this.get(1),this.get(2),this.get(3),this.get(4),this.get(5)];a.scalex=+a.scalex.toFixed(4);a.scaley=+a.scaley.toFixed(4);a.rotate=+a.rotate.toFixed(4);return(a.dx||a.dy?"t"+[+a.dx.toFixed(4),+a.dy.toFixed(4)]:"")+(1!=a.scalex||1!=a.scaley?"s"+[a.scalex,a.scaley,0,0]:"")+(a.rotate?"r"+[+a.rotate.toFixed(4),0,0]:"")}})(w.prototype);a.Matrix=w;a.matrix=function(a,d,f,b,k,e){return new w(a,d,f,b,k,e)}});C.plugin(function(a,v,y,M,A){function w(h){return function(d){k.stop();d instanceof A&&1==d.node.childNodes.length&&
("radialGradient"==d.node.firstChild.tagName||"linearGradient"==d.node.firstChild.tagName||"pattern"==d.node.firstChild.tagName)&&(d=d.node.firstChild,b(this).appendChild(d),d=u(d));if(d instanceof v)if("radialGradient"==d.type||"linearGradient"==d.type||"pattern"==d.type){d.node.id||e(d.node,{id:d.id});var f=l(d.node.id)}else f=d.attr(h);else f=a.color(d),f.error?(f=a(b(this).ownerSVGElement).gradient(d))?(f.node.id||e(f.node,{id:f.id}),f=l(f.node.id)):f=d:f=r(f);d={};d[h]=f;e(this.node,d);this.node.style[h]=
x}}function z(a){k.stop();a==+a&&(a+="px");this.node.style.fontSize=a}function d(a){var b=[];a=a.childNodes;for(var e=0,f=a.length;e<f;e++){var l=a[e];3==l.nodeType&&b.push(l.nodeValue);"tspan"==l.tagName&&(1==l.childNodes.length&&3==l.firstChild.nodeType?b.push(l.firstChild.nodeValue):b.push(d(l)))}return b}function f(){k.stop();return this.node.style.fontSize}var n=a._.make,u=a._.wrap,p=a.is,b=a._.getSomeDefs,q=/^url\(#?([^)]+)\)$/,e=a._.$,l=a.url,r=String,s=a._.separator,x="";k.on("snap.util.attr.mask",
function(a){if(a instanceof v||a instanceof A){k.stop();a instanceof A&&1==a.node.childNodes.length&&(a=a.node.firstChild,b(this).appendChild(a),a=u(a));if("mask"==a.type)var d=a;else d=n("mask",b(this)),d.node.appendChild(a.node);!d.node.id&&e(d.node,{id:d.id});e(this.node,{mask:l(d.id)})}});(function(a){k.on("snap.util.attr.clip",a);k.on("snap.util.attr.clip-path",a);k.on("snap.util.attr.clipPath",a)})(function(a){if(a instanceof v||a instanceof A){k.stop();if("clipPath"==a.type)var d=a;else d=
n("clipPath",b(this)),d.node.appendChild(a.node),!d.node.id&&e(d.node,{id:d.id});e(this.node,{"clip-path":l(d.id)})}});k.on("snap.util.attr.fill",w("fill"));k.on("snap.util.attr.stroke",w("stroke"));var G=/^([lr])(?:\(([^)]*)\))?(.*)$/i;k.on("snap.util.grad.parse",function(a){a=r(a);var b=a.match(G);if(!b)return null;a=b[1];var e=b[2],b=b[3],e=e.split(/\s*,\s*/).map(function(a){return+a==a?+a:a});1==e.length&&0==e[0]&&(e=[]);b=b.split("-");b=b.map(function(a){a=a.split(":");var b={color:a[0]};a[1]&&
(b.offset=parseFloat(a[1]));return b});return{type:a,params:e,stops:b}});k.on("snap.util.attr.d",function(b){k.stop();p(b,"array")&&p(b[0],"array")&&(b=a.path.toString.call(b));b=r(b);b.match(/[ruo]/i)&&(b=a.path.toAbsolute(b));e(this.node,{d:b})})(-1);k.on("snap.util.attr.#text",function(a){k.stop();a=r(a);for(a=M.doc.createTextNode(a);this.node.firstChild;)this.node.removeChild(this.node.firstChild);this.node.appendChild(a)})(-1);k.on("snap.util.attr.path",function(a){k.stop();this.attr({d:a})})(-1);
k.on("snap.util.attr.class",function(a){k.stop();this.node.className.baseVal=a})(-1);k.on("snap.util.attr.viewBox",function(a){a=p(a,"object")&&"x"in a?[a.x,a.y,a.width,a.height].join(" "):p(a,"array")?a.join(" "):a;e(this.node,{viewBox:a});k.stop()})(-1);k.on("snap.util.attr.transform",function(a){this.transform(a);k.stop()})(-1);k.on("snap.util.attr.r",function(a){"rect"==this.type&&(k.stop(),e(this.node,{rx:a,ry:a}))})(-1);k.on("snap.util.attr.textpath",function(a){k.stop();if("text"==this.type){var d,
f;if(!a&&this.textPath){for(a=this.textPath;a.node.firstChild;)this.node.appendChild(a.node.firstChild);a.remove();delete this.textPath}else if(p(a,"string")?(d=b(this),a=u(d.parentNode).path(a),d.appendChild(a.node),d=a.id,a.attr({id:d})):(a=u(a),a instanceof v&&(d=a.attr("id"),d||(d=a.id,a.attr({id:d})))),d)if(a=this.textPath,f=this.node,a)a.attr({"xlink:href":"#"+d});else{for(a=e("textPath",{"xlink:href":"#"+d});f.firstChild;)a.appendChild(f.firstChild);f.appendChild(a);this.textPath=u(a)}}})(-1);
k.on("snap.util.attr.text",function(a){if("text"==this.type){for(var b=this.node,d=function(a){var b=e("tspan");if(p(a,"array"))for(var f=0;f<a.length;f++)b.appendChild(d(a[f]));else b.appendChild(M.doc.createTextNode(a));b.normalize&&b.normalize();return b};b.firstChild;)b.removeChild(b.firstChild);for(a=d(a);a.firstChild;)b.appendChild(a.firstChild)}k.stop()})(-1);k.on("snap.util.attr.fontSize",z)(-1);k.on("snap.util.attr.font-size",z)(-1);k.on("snap.util.getattr.transform",function(){k.stop();
return this.transform()})(-1);k.on("snap.util.getattr.textpath",function(){k.stop();return this.textPath})(-1);(function(){function b(d){return function(){k.stop();var b=M.doc.defaultView.getComputedStyle(this.node,null).getPropertyValue("marker-"+d);return"none"==b?b:a(M.doc.getElementById(b.match(q)[1]))}}function d(a){return function(b){k.stop();var d="marker"+a.charAt(0).toUpperCase()+a.substring(1);if(""==b||!b)this.node.style[d]="none";else if("marker"==b.type){var f=b.node.id;f||e(b.node,{id:b.id});
this.node.style[d]=l(f)}}}k.on("snap.util.getattr.marker-end",b("end"))(-1);k.on("snap.util.getattr.markerEnd",b("end"))(-1);k.on("snap.util.getattr.marker-start",b("start"))(-1);k.on("snap.util.getattr.markerStart",b("start"))(-1);k.on("snap.util.getattr.marker-mid",b("mid"))(-1);k.on("snap.util.getattr.markerMid",b("mid"))(-1);k.on("snap.util.attr.marker-end",d("end"))(-1);k.on("snap.util.attr.markerEnd",d("end"))(-1);k.on("snap.util.attr.marker-start",d("start"))(-1);k.on("snap.util.attr.markerStart",
d("start"))(-1);k.on("snap.util.attr.marker-mid",d("mid"))(-1);k.on("snap.util.attr.markerMid",d("mid"))(-1)})();k.on("snap.util.getattr.r",function(){if("rect"==this.type&&e(this.node,"rx")==e(this.node,"ry"))return k.stop(),e(this.node,"rx")})(-1);k.on("snap.util.getattr.text",function(){if("text"==this.type||"tspan"==this.type){k.stop();var a=d(this.node);return 1==a.length?a[0]:a}})(-1);k.on("snap.util.getattr.#text",function(){return this.node.textContent})(-1);k.on("snap.util.getattr.viewBox",
function(){k.stop();var b=e(this.node,"viewBox");if(b)return b=b.split(s),a._.box(+b[0],+b[1],+b[2],+b[3])})(-1);k.on("snap.util.getattr.points",function(){var a=e(this.node,"points");k.stop();if(a)return a.split(s)})(-1);k.on("snap.util.getattr.path",function(){var a=e(this.node,"d");k.stop();return a})(-1);k.on("snap.util.getattr.class",function(){return this.node.className.baseVal})(-1);k.on("snap.util.getattr.fontSize",f)(-1);k.on("snap.util.getattr.font-size",f)(-1)});C.plugin(function(a,v,y,
M,A){function w(a){return a}function z(a){return function(b){return+b.toFixed(3)+a}}var d={"+":function(a,b){return a+b},"-":function(a,b){return a-b},"/":function(a,b){return a/b},"*":function(a,b){return a*b}},f=String,n=/[a-z]+$/i,u=/^\s*([+\-\/*])\s*=\s*([\d.eE+\-]+)\s*([^\d\s]+)?\s*$/;k.on("snap.util.attr",function(a){if(a=f(a).match(u)){var b=k.nt(),b=b.substring(b.lastIndexOf(".")+1),q=this.attr(b),e={};k.stop();var l=a[3]||"",r=q.match(n),s=d[a[1] ];r&&r==l?a=s(parseFloat(q),+a[2]):(q=this.asPX(b),
a=s(this.asPX(b),this.asPX(b,a[2]+l)));isNaN(q)||isNaN(a)||(e[b]=a,this.attr(e))}})(-10);k.on("snap.util.equal",function(a,b){var q=f(this.attr(a)||""),e=f(b).match(u);if(e){k.stop();var l=e[3]||"",r=q.match(n),s=d[e[1] ];if(r&&r==l)return{from:parseFloat(q),to:s(parseFloat(q),+e[2]),f:z(r)};q=this.asPX(a);return{from:q,to:s(q,this.asPX(a,e[2]+l)),f:w}}})(-10)});C.plugin(function(a,v,y,M,A){var w=y.prototype,z=a.is;w.rect=function(a,d,k,p,b,q){var e;null==q&&(q=b);z(a,"object")&&"[object Object]"==
a?e=a:null!=a&&(e={x:a,y:d,width:k,height:p},null!=b&&(e.rx=b,e.ry=q));return this.el("rect",e)};w.circle=function(a,d,k){var p;z(a,"object")&&"[object Object]"==a?p=a:null!=a&&(p={cx:a,cy:d,r:k});return this.el("circle",p)};var d=function(){function a(){this.parentNode.removeChild(this)}return function(d,k){var p=M.doc.createElement("img"),b=M.doc.body;p.style.cssText="position:absolute;left:-9999em;top:-9999em";p.onload=function(){k.call(p);p.onload=p.onerror=null;b.removeChild(p)};p.onerror=a;
b.appendChild(p);p.src=d}}();w.image=function(f,n,k,p,b){var q=this.el("image");if(z(f,"object")&&"src"in f)q.attr(f);else if(null!=f){var e={"xlink:href":f,preserveAspectRatio:"none"};null!=n&&null!=k&&(e.x=n,e.y=k);null!=p&&null!=b?(e.width=p,e.height=b):d(f,function(){a._.$(q.node,{width:this.offsetWidth,height:this.offsetHeight})});a._.$(q.node,e)}return q};w.ellipse=function(a,d,k,p){var b;z(a,"object")&&"[object Object]"==a?b=a:null!=a&&(b={cx:a,cy:d,rx:k,ry:p});return this.el("ellipse",b)};
w.path=function(a){var d;z(a,"object")&&!z(a,"array")?d=a:a&&(d={d:a});return this.el("path",d)};w.group=w.g=function(a){var d=this.el("g");1==arguments.length&&a&&!a.type?d.attr(a):arguments.length&&d.add(Array.prototype.slice.call(arguments,0));return d};w.svg=function(a,d,k,p,b,q,e,l){var r={};z(a,"object")&&null==d?r=a:(null!=a&&(r.x=a),null!=d&&(r.y=d),null!=k&&(r.width=k),null!=p&&(r.height=p),null!=b&&null!=q&&null!=e&&null!=l&&(r.viewBox=[b,q,e,l]));return this.el("svg",r)};w.mask=function(a){var d=
this.el("mask");1==arguments.length&&a&&!a.type?d.attr(a):arguments.length&&d.add(Array.prototype.slice.call(arguments,0));return d};w.ptrn=function(a,d,k,p,b,q,e,l){if(z(a,"object"))var r=a;else arguments.length?(r={},null!=a&&(r.x=a),null!=d&&(r.y=d),null!=k&&(r.width=k),null!=p&&(r.height=p),null!=b&&null!=q&&null!=e&&null!=l&&(r.viewBox=[b,q,e,l])):r={patternUnits:"userSpaceOnUse"};return this.el("pattern",r)};w.use=function(a){return null!=a?(make("use",this.node),a instanceof v&&(a.attr("id")||
a.attr({id:ID()}),a=a.attr("id")),this.el("use",{"xlink:href":a})):v.prototype.use.call(this)};w.text=function(a,d,k){var p={};z(a,"object")?p=a:null!=a&&(p={x:a,y:d,text:k||""});return this.el("text",p)};w.line=function(a,d,k,p){var b={};z(a,"object")?b=a:null!=a&&(b={x1:a,x2:k,y1:d,y2:p});return this.el("line",b)};w.polyline=function(a){1<arguments.length&&(a=Array.prototype.slice.call(arguments,0));var d={};z(a,"object")&&!z(a,"array")?d=a:null!=a&&(d={points:a});return this.el("polyline",d)};
w.polygon=function(a){1<arguments.length&&(a=Array.prototype.slice.call(arguments,0));var d={};z(a,"object")&&!z(a,"array")?d=a:null!=a&&(d={points:a});return this.el("polygon",d)};(function(){function d(){return this.selectAll("stop")}function n(b,d){var f=e("stop"),k={offset:+d+"%"};b=a.color(b);k["stop-color"]=b.hex;1>b.opacity&&(k["stop-opacity"]=b.opacity);e(f,k);this.node.appendChild(f);return this}function u(){if("linearGradient"==this.type){var b=e(this.node,"x1")||0,d=e(this.node,"x2")||
1,f=e(this.node,"y1")||0,k=e(this.node,"y2")||0;return a._.box(b,f,math.abs(d-b),math.abs(k-f))}b=this.node.r||0;return a._.box((this.node.cx||0.5)-b,(this.node.cy||0.5)-b,2*b,2*b)}function p(a,d){function f(a,b){for(var d=(b-u)/(a-w),e=w;e<a;e++)h[e].offset=+(+u+d*(e-w)).toFixed(2);w=a;u=b}var n=k("snap.util.grad.parse",null,d).firstDefined(),p;if(!n)return null;n.params.unshift(a);p="l"==n.type.toLowerCase()?b.apply(0,n.params):q.apply(0,n.params);n.type!=n.type.toLowerCase()&&e(p.node,{gradientUnits:"userSpaceOnUse"});
var h=n.stops,n=h.length,u=0,w=0;n--;for(var v=0;v<n;v++)"offset"in h[v]&&f(v,h[v].offset);h[n].offset=h[n].offset||100;f(n,h[n].offset);for(v=0;v<=n;v++){var y=h[v];p.addStop(y.color,y.offset)}return p}function b(b,k,p,q,w){b=a._.make("linearGradient",b);b.stops=d;b.addStop=n;b.getBBox=u;null!=k&&e(b.node,{x1:k,y1:p,x2:q,y2:w});return b}function q(b,k,p,q,w,h){b=a._.make("radialGradient",b);b.stops=d;b.addStop=n;b.getBBox=u;null!=k&&e(b.node,{cx:k,cy:p,r:q});null!=w&&null!=h&&e(b.node,{fx:w,fy:h});
return b}var e=a._.$;w.gradient=function(a){return p(this.defs,a)};w.gradientLinear=function(a,d,e,f){return b(this.defs,a,d,e,f)};w.gradientRadial=function(a,b,d,e,f){return q(this.defs,a,b,d,e,f)};w.toString=function(){var b=this.node.ownerDocument,d=b.createDocumentFragment(),b=b.createElement("div"),e=this.node.cloneNode(!0);d.appendChild(b);b.appendChild(e);a._.$(e,{xmlns:"http://www.w3.org/2000/svg"});b=b.innerHTML;d.removeChild(d.firstChild);return b};w.clear=function(){for(var a=this.node.firstChild,
b;a;)b=a.nextSibling,"defs"!=a.tagName?a.parentNode.removeChild(a):w.clear.call({node:a}),a=b}})()});C.plugin(function(a,k,y,M){function A(a){var b=A.ps=A.ps||{};b[a]?b[a].sleep=100:b[a]={sleep:100};setTimeout(function(){for(var d in b)b[L](d)&&d!=a&&(b[d].sleep--,!b[d].sleep&&delete b[d])});return b[a]}function w(a,b,d,e){null==a&&(a=b=d=e=0);null==b&&(b=a.y,d=a.width,e=a.height,a=a.x);return{x:a,y:b,width:d,w:d,height:e,h:e,x2:a+d,y2:b+e,cx:a+d/2,cy:b+e/2,r1:F.min(d,e)/2,r2:F.max(d,e)/2,r0:F.sqrt(d*
d+e*e)/2,path:s(a,b,d,e),vb:[a,b,d,e].join(" ")}}function z(){return this.join(",").replace(N,"$1")}function d(a){a=C(a);a.toString=z;return a}function f(a,b,d,h,f,k,l,n,p){if(null==p)return e(a,b,d,h,f,k,l,n);if(0>p||e(a,b,d,h,f,k,l,n)<p)p=void 0;else{var q=0.5,O=1-q,s;for(s=e(a,b,d,h,f,k,l,n,O);0.01<Z(s-p);)q/=2,O+=(s<p?1:-1)*q,s=e(a,b,d,h,f,k,l,n,O);p=O}return u(a,b,d,h,f,k,l,n,p)}function n(b,d){function e(a){return+(+a).toFixed(3)}return a._.cacher(function(a,h,l){a instanceof k&&(a=a.attr("d"));
a=I(a);for(var n,p,D,q,O="",s={},c=0,t=0,r=a.length;t<r;t++){D=a[t];if("M"==D[0])n=+D[1],p=+D[2];else{q=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6]);if(c+q>h){if(d&&!s.start){n=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6],h-c);O+=["C"+e(n.start.x),e(n.start.y),e(n.m.x),e(n.m.y),e(n.x),e(n.y)];if(l)return O;s.start=O;O=["M"+e(n.x),e(n.y)+"C"+e(n.n.x),e(n.n.y),e(n.end.x),e(n.end.y),e(D[5]),e(D[6])].join();c+=q;n=+D[5];p=+D[6];continue}if(!b&&!d)return n=f(n,p,D[1],D[2],D[3],D[4],D[5],D[6],h-c)}c+=q;n=+D[5];p=+D[6]}O+=
D.shift()+D}s.end=O;return n=b?c:d?s:u(n,p,D[0],D[1],D[2],D[3],D[4],D[5],1)},null,a._.clone)}function u(a,b,d,e,h,f,k,l,n){var p=1-n,q=ma(p,3),s=ma(p,2),c=n*n,t=c*n,r=q*a+3*s*n*d+3*p*n*n*h+t*k,q=q*b+3*s*n*e+3*p*n*n*f+t*l,s=a+2*n*(d-a)+c*(h-2*d+a),t=b+2*n*(e-b)+c*(f-2*e+b),x=d+2*n*(h-d)+c*(k-2*h+d),c=e+2*n*(f-e)+c*(l-2*f+e);a=p*a+n*d;b=p*b+n*e;h=p*h+n*k;f=p*f+n*l;l=90-180*F.atan2(s-x,t-c)/S;return{x:r,y:q,m:{x:s,y:t},n:{x:x,y:c},start:{x:a,y:b},end:{x:h,y:f},alpha:l}}function p(b,d,e,h,f,n,k,l){a.is(b,
"array")||(b=[b,d,e,h,f,n,k,l]);b=U.apply(null,b);return w(b.min.x,b.min.y,b.max.x-b.min.x,b.max.y-b.min.y)}function b(a,b,d){return b>=a.x&&b<=a.x+a.width&&d>=a.y&&d<=a.y+a.height}function q(a,d){a=w(a);d=w(d);return b(d,a.x,a.y)||b(d,a.x2,a.y)||b(d,a.x,a.y2)||b(d,a.x2,a.y2)||b(a,d.x,d.y)||b(a,d.x2,d.y)||b(a,d.x,d.y2)||b(a,d.x2,d.y2)||(a.x<d.x2&&a.x>d.x||d.x<a.x2&&d.x>a.x)&&(a.y<d.y2&&a.y>d.y||d.y<a.y2&&d.y>a.y)}function e(a,b,d,e,h,f,n,k,l){null==l&&(l=1);l=(1<l?1:0>l?0:l)/2;for(var p=[-0.1252,
0.1252,-0.3678,0.3678,-0.5873,0.5873,-0.7699,0.7699,-0.9041,0.9041,-0.9816,0.9816],q=[0.2491,0.2491,0.2335,0.2335,0.2032,0.2032,0.1601,0.1601,0.1069,0.1069,0.0472,0.0472],s=0,c=0;12>c;c++)var t=l*p[c]+l,r=t*(t*(-3*a+9*d-9*h+3*n)+6*a-12*d+6*h)-3*a+3*d,t=t*(t*(-3*b+9*e-9*f+3*k)+6*b-12*e+6*f)-3*b+3*e,s=s+q[c]*F.sqrt(r*r+t*t);return l*s}function l(a,b,d){a=I(a);b=I(b);for(var h,f,l,n,k,s,r,O,x,c,t=d?0:[],w=0,v=a.length;w<v;w++)if(x=a[w],"M"==x[0])h=k=x[1],f=s=x[2];else{"C"==x[0]?(x=[h,f].concat(x.slice(1)),
h=x[6],f=x[7]):(x=[h,f,h,f,k,s,k,s],h=k,f=s);for(var G=0,y=b.length;G<y;G++)if(c=b[G],"M"==c[0])l=r=c[1],n=O=c[2];else{"C"==c[0]?(c=[l,n].concat(c.slice(1)),l=c[6],n=c[7]):(c=[l,n,l,n,r,O,r,O],l=r,n=O);var z;var K=x,B=c;z=d;var H=p(K),J=p(B);if(q(H,J)){for(var H=e.apply(0,K),J=e.apply(0,B),H=~~(H/8),J=~~(J/8),U=[],A=[],F={},M=z?0:[],P=0;P<H+1;P++){var C=u.apply(0,K.concat(P/H));U.push({x:C.x,y:C.y,t:P/H})}for(P=0;P<J+1;P++)C=u.apply(0,B.concat(P/J)),A.push({x:C.x,y:C.y,t:P/J});for(P=0;P<H;P++)for(K=
0;K<J;K++){var Q=U[P],L=U[P+1],B=A[K],C=A[K+1],N=0.001>Z(L.x-Q.x)?"y":"x",S=0.001>Z(C.x-B.x)?"y":"x",R;R=Q.x;var Y=Q.y,V=L.x,ea=L.y,fa=B.x,ga=B.y,ha=C.x,ia=C.y;if(W(R,V)<X(fa,ha)||X(R,V)>W(fa,ha)||W(Y,ea)<X(ga,ia)||X(Y,ea)>W(ga,ia))R=void 0;else{var $=(R*ea-Y*V)*(fa-ha)-(R-V)*(fa*ia-ga*ha),aa=(R*ea-Y*V)*(ga-ia)-(Y-ea)*(fa*ia-ga*ha),ja=(R-V)*(ga-ia)-(Y-ea)*(fa-ha);if(ja){var $=$/ja,aa=aa/ja,ja=+$.toFixed(2),ba=+aa.toFixed(2);R=ja<+X(R,V).toFixed(2)||ja>+W(R,V).toFixed(2)||ja<+X(fa,ha).toFixed(2)||
ja>+W(fa,ha).toFixed(2)||ba<+X(Y,ea).toFixed(2)||ba>+W(Y,ea).toFixed(2)||ba<+X(ga,ia).toFixed(2)||ba>+W(ga,ia).toFixed(2)?void 0:{x:$,y:aa}}else R=void 0}R&&F[R.x.toFixed(4)]!=R.y.toFixed(4)&&(F[R.x.toFixed(4)]=R.y.toFixed(4),Q=Q.t+Z((R[N]-Q[N])/(L[N]-Q[N]))*(L.t-Q.t),B=B.t+Z((R[S]-B[S])/(C[S]-B[S]))*(C.t-B.t),0<=Q&&1>=Q&&0<=B&&1>=B&&(z?M++:M.push({x:R.x,y:R.y,t1:Q,t2:B})))}z=M}else z=z?0:[];if(d)t+=z;else{H=0;for(J=z.length;H<J;H++)z[H].segment1=w,z[H].segment2=G,z[H].bez1=x,z[H].bez2=c;t=t.concat(z)}}}return t}
function r(a){var b=A(a);if(b.bbox)return C(b.bbox);if(!a)return w();a=I(a);for(var d=0,e=0,h=[],f=[],l,n=0,k=a.length;n<k;n++)l=a[n],"M"==l[0]?(d=l[1],e=l[2],h.push(d),f.push(e)):(d=U(d,e,l[1],l[2],l[3],l[4],l[5],l[6]),h=h.concat(d.min.x,d.max.x),f=f.concat(d.min.y,d.max.y),d=l[5],e=l[6]);a=X.apply(0,h);l=X.apply(0,f);h=W.apply(0,h);f=W.apply(0,f);f=w(a,l,h-a,f-l);b.bbox=C(f);return f}function s(a,b,d,e,h){if(h)return[["M",+a+ +h,b],["l",d-2*h,0],["a",h,h,0,0,1,h,h],["l",0,e-2*h],["a",h,h,0,0,1,
-h,h],["l",2*h-d,0],["a",h,h,0,0,1,-h,-h],["l",0,2*h-e],["a",h,h,0,0,1,h,-h],["z"] ];a=[["M",a,b],["l",d,0],["l",0,e],["l",-d,0],["z"] ];a.toString=z;return a}function x(a,b,d,e,h){null==h&&null==e&&(e=d);a=+a;b=+b;d=+d;e=+e;if(null!=h){var f=Math.PI/180,l=a+d*Math.cos(-e*f);a+=d*Math.cos(-h*f);var n=b+d*Math.sin(-e*f);b+=d*Math.sin(-h*f);d=[["M",l,n],["A",d,d,0,+(180<h-e),0,a,b] ]}else d=[["M",a,b],["m",0,-e],["a",d,e,0,1,1,0,2*e],["a",d,e,0,1,1,0,-2*e],["z"] ];d.toString=z;return d}function G(b){var e=
A(b);if(e.abs)return d(e.abs);Q(b,"array")&&Q(b&&b[0],"array")||(b=a.parsePathString(b));if(!b||!b.length)return[["M",0,0] ];var h=[],f=0,l=0,n=0,k=0,p=0;"M"==b[0][0]&&(f=+b[0][1],l=+b[0][2],n=f,k=l,p++,h[0]=["M",f,l]);for(var q=3==b.length&&"M"==b[0][0]&&"R"==b[1][0].toUpperCase()&&"Z"==b[2][0].toUpperCase(),s,r,w=p,c=b.length;w<c;w++){h.push(s=[]);r=b[w];p=r[0];if(p!=p.toUpperCase())switch(s[0]=p.toUpperCase(),s[0]){case "A":s[1]=r[1];s[2]=r[2];s[3]=r[3];s[4]=r[4];s[5]=r[5];s[6]=+r[6]+f;s[7]=+r[7]+
l;break;case "V":s[1]=+r[1]+l;break;case "H":s[1]=+r[1]+f;break;case "R":for(var t=[f,l].concat(r.slice(1)),u=2,v=t.length;u<v;u++)t[u]=+t[u]+f,t[++u]=+t[u]+l;h.pop();h=h.concat(P(t,q));break;case "O":h.pop();t=x(f,l,r[1],r[2]);t.push(t[0]);h=h.concat(t);break;case "U":h.pop();h=h.concat(x(f,l,r[1],r[2],r[3]));s=["U"].concat(h[h.length-1].slice(-2));break;case "M":n=+r[1]+f,k=+r[2]+l;default:for(u=1,v=r.length;u<v;u++)s[u]=+r[u]+(u%2?f:l)}else if("R"==p)t=[f,l].concat(r.slice(1)),h.pop(),h=h.concat(P(t,
q)),s=["R"].concat(r.slice(-2));else if("O"==p)h.pop(),t=x(f,l,r[1],r[2]),t.push(t[0]),h=h.concat(t);else if("U"==p)h.pop(),h=h.concat(x(f,l,r[1],r[2],r[3])),s=["U"].concat(h[h.length-1].slice(-2));else for(t=0,u=r.length;t<u;t++)s[t]=r[t];p=p.toUpperCase();if("O"!=p)switch(s[0]){case "Z":f=+n;l=+k;break;case "H":f=s[1];break;case "V":l=s[1];break;case "M":n=s[s.length-2],k=s[s.length-1];default:f=s[s.length-2],l=s[s.length-1]}}h.toString=z;e.abs=d(h);return h}function h(a,b,d,e){return[a,b,d,e,d,
e]}function J(a,b,d,e,h,f){var l=1/3,n=2/3;return[l*a+n*d,l*b+n*e,l*h+n*d,l*f+n*e,h,f]}function K(b,d,e,h,f,l,n,k,p,s){var r=120*S/180,q=S/180*(+f||0),c=[],t,x=a._.cacher(function(a,b,c){var d=a*F.cos(c)-b*F.sin(c);a=a*F.sin(c)+b*F.cos(c);return{x:d,y:a}});if(s)v=s[0],t=s[1],l=s[2],u=s[3];else{t=x(b,d,-q);b=t.x;d=t.y;t=x(k,p,-q);k=t.x;p=t.y;F.cos(S/180*f);F.sin(S/180*f);t=(b-k)/2;v=(d-p)/2;u=t*t/(e*e)+v*v/(h*h);1<u&&(u=F.sqrt(u),e*=u,h*=u);var u=e*e,w=h*h,u=(l==n?-1:1)*F.sqrt(Z((u*w-u*v*v-w*t*t)/
(u*v*v+w*t*t)));l=u*e*v/h+(b+k)/2;var u=u*-h*t/e+(d+p)/2,v=F.asin(((d-u)/h).toFixed(9));t=F.asin(((p-u)/h).toFixed(9));v=b<l?S-v:v;t=k<l?S-t:t;0>v&&(v=2*S+v);0>t&&(t=2*S+t);n&&v>t&&(v-=2*S);!n&&t>v&&(t-=2*S)}if(Z(t-v)>r){var c=t,w=k,G=p;t=v+r*(n&&t>v?1:-1);k=l+e*F.cos(t);p=u+h*F.sin(t);c=K(k,p,e,h,f,0,n,w,G,[t,c,l,u])}l=t-v;f=F.cos(v);r=F.sin(v);n=F.cos(t);t=F.sin(t);l=F.tan(l/4);e=4/3*e*l;l*=4/3*h;h=[b,d];b=[b+e*r,d-l*f];d=[k+e*t,p-l*n];k=[k,p];b[0]=2*h[0]-b[0];b[1]=2*h[1]-b[1];if(s)return[b,d,k].concat(c);
c=[b,d,k].concat(c).join().split(",");s=[];k=0;for(p=c.length;k<p;k++)s[k]=k%2?x(c[k-1],c[k],q).y:x(c[k],c[k+1],q).x;return s}function U(a,b,d,e,h,f,l,k){for(var n=[],p=[[],[] ],s,r,c,t,q=0;2>q;++q)0==q?(r=6*a-12*d+6*h,s=-3*a+9*d-9*h+3*l,c=3*d-3*a):(r=6*b-12*e+6*f,s=-3*b+9*e-9*f+3*k,c=3*e-3*b),1E-12>Z(s)?1E-12>Z(r)||(s=-c/r,0<s&&1>s&&n.push(s)):(t=r*r-4*c*s,c=F.sqrt(t),0>t||(t=(-r+c)/(2*s),0<t&&1>t&&n.push(t),s=(-r-c)/(2*s),0<s&&1>s&&n.push(s)));for(r=q=n.length;q--;)s=n[q],c=1-s,p[0][q]=c*c*c*a+3*
c*c*s*d+3*c*s*s*h+s*s*s*l,p[1][q]=c*c*c*b+3*c*c*s*e+3*c*s*s*f+s*s*s*k;p[0][r]=a;p[1][r]=b;p[0][r+1]=l;p[1][r+1]=k;p[0].length=p[1].length=r+2;return{min:{x:X.apply(0,p[0]),y:X.apply(0,p[1])},max:{x:W.apply(0,p[0]),y:W.apply(0,p[1])}}}function I(a,b){var e=!b&&A(a);if(!b&&e.curve)return d(e.curve);var f=G(a),l=b&&G(b),n={x:0,y:0,bx:0,by:0,X:0,Y:0,qx:null,qy:null},k={x:0,y:0,bx:0,by:0,X:0,Y:0,qx:null,qy:null},p=function(a,b,c){if(!a)return["C",b.x,b.y,b.x,b.y,b.x,b.y];a[0]in{T:1,Q:1}||(b.qx=b.qy=null);
switch(a[0]){case "M":b.X=a[1];b.Y=a[2];break;case "A":a=["C"].concat(K.apply(0,[b.x,b.y].concat(a.slice(1))));break;case "S":"C"==c||"S"==c?(c=2*b.x-b.bx,b=2*b.y-b.by):(c=b.x,b=b.y);a=["C",c,b].concat(a.slice(1));break;case "T":"Q"==c||"T"==c?(b.qx=2*b.x-b.qx,b.qy=2*b.y-b.qy):(b.qx=b.x,b.qy=b.y);a=["C"].concat(J(b.x,b.y,b.qx,b.qy,a[1],a[2]));break;case "Q":b.qx=a[1];b.qy=a[2];a=["C"].concat(J(b.x,b.y,a[1],a[2],a[3],a[4]));break;case "L":a=["C"].concat(h(b.x,b.y,a[1],a[2]));break;case "H":a=["C"].concat(h(b.x,
b.y,a[1],b.y));break;case "V":a=["C"].concat(h(b.x,b.y,b.x,a[1]));break;case "Z":a=["C"].concat(h(b.x,b.y,b.X,b.Y))}return a},s=function(a,b){if(7<a[b].length){a[b].shift();for(var c=a[b];c.length;)q[b]="A",l&&(u[b]="A"),a.splice(b++,0,["C"].concat(c.splice(0,6)));a.splice(b,1);v=W(f.length,l&&l.length||0)}},r=function(a,b,c,d,e){a&&b&&"M"==a[e][0]&&"M"!=b[e][0]&&(b.splice(e,0,["M",d.x,d.y]),c.bx=0,c.by=0,c.x=a[e][1],c.y=a[e][2],v=W(f.length,l&&l.length||0))},q=[],u=[],c="",t="",x=0,v=W(f.length,
l&&l.length||0);for(;x<v;x++){f[x]&&(c=f[x][0]);"C"!=c&&(q[x]=c,x&&(t=q[x-1]));f[x]=p(f[x],n,t);"A"!=q[x]&&"C"==c&&(q[x]="C");s(f,x);l&&(l[x]&&(c=l[x][0]),"C"!=c&&(u[x]=c,x&&(t=u[x-1])),l[x]=p(l[x],k,t),"A"!=u[x]&&"C"==c&&(u[x]="C"),s(l,x));r(f,l,n,k,x);r(l,f,k,n,x);var w=f[x],z=l&&l[x],y=w.length,U=l&&z.length;n.x=w[y-2];n.y=w[y-1];n.bx=$(w[y-4])||n.x;n.by=$(w[y-3])||n.y;k.bx=l&&($(z[U-4])||k.x);k.by=l&&($(z[U-3])||k.y);k.x=l&&z[U-2];k.y=l&&z[U-1]}l||(e.curve=d(f));return l?[f,l]:f}function P(a,
b){for(var d=[],e=0,h=a.length;h-2*!b>e;e+=2){var f=[{x:+a[e-2],y:+a[e-1]},{x:+a[e],y:+a[e+1]},{x:+a[e+2],y:+a[e+3]},{x:+a[e+4],y:+a[e+5]}];b?e?h-4==e?f[3]={x:+a[0],y:+a[1]}:h-2==e&&(f[2]={x:+a[0],y:+a[1]},f[3]={x:+a[2],y:+a[3]}):f[0]={x:+a[h-2],y:+a[h-1]}:h-4==e?f[3]=f[2]:e||(f[0]={x:+a[e],y:+a[e+1]});d.push(["C",(-f[0].x+6*f[1].x+f[2].x)/6,(-f[0].y+6*f[1].y+f[2].y)/6,(f[1].x+6*f[2].x-f[3].x)/6,(f[1].y+6*f[2].y-f[3].y)/6,f[2].x,f[2].y])}return d}y=k.prototype;var Q=a.is,C=a._.clone,L="hasOwnProperty",
N=/,?([a-z]),?/gi,$=parseFloat,F=Math,S=F.PI,X=F.min,W=F.max,ma=F.pow,Z=F.abs;M=n(1);var na=n(),ba=n(0,1),V=a._unit2px;a.path=A;a.path.getTotalLength=M;a.path.getPointAtLength=na;a.path.getSubpath=function(a,b,d){if(1E-6>this.getTotalLength(a)-d)return ba(a,b).end;a=ba(a,d,1);return b?ba(a,b).end:a};y.getTotalLength=function(){if(this.node.getTotalLength)return this.node.getTotalLength()};y.getPointAtLength=function(a){return na(this.attr("d"),a)};y.getSubpath=function(b,d){return a.path.getSubpath(this.attr("d"),
b,d)};a._.box=w;a.path.findDotsAtSegment=u;a.path.bezierBBox=p;a.path.isPointInsideBBox=b;a.path.isBBoxIntersect=q;a.path.intersection=function(a,b){return l(a,b)};a.path.intersectionNumber=function(a,b){return l(a,b,1)};a.path.isPointInside=function(a,d,e){var h=r(a);return b(h,d,e)&&1==l(a,[["M",d,e],["H",h.x2+10] ],1)%2};a.path.getBBox=r;a.path.get={path:function(a){return a.attr("path")},circle:function(a){a=V(a);return x(a.cx,a.cy,a.r)},ellipse:function(a){a=V(a);return x(a.cx||0,a.cy||0,a.rx,
a.ry)},rect:function(a){a=V(a);return s(a.x||0,a.y||0,a.width,a.height,a.rx,a.ry)},image:function(a){a=V(a);return s(a.x||0,a.y||0,a.width,a.height)},line:function(a){return"M"+[a.attr("x1")||0,a.attr("y1")||0,a.attr("x2"),a.attr("y2")]},polyline:function(a){return"M"+a.attr("points")},polygon:function(a){return"M"+a.attr("points")+"z"},deflt:function(a){a=a.node.getBBox();return s(a.x,a.y,a.width,a.height)}};a.path.toRelative=function(b){var e=A(b),h=String.prototype.toLowerCase;if(e.rel)return d(e.rel);
a.is(b,"array")&&a.is(b&&b[0],"array")||(b=a.parsePathString(b));var f=[],l=0,n=0,k=0,p=0,s=0;"M"==b[0][0]&&(l=b[0][1],n=b[0][2],k=l,p=n,s++,f.push(["M",l,n]));for(var r=b.length;s<r;s++){var q=f[s]=[],x=b[s];if(x[0]!=h.call(x[0]))switch(q[0]=h.call(x[0]),q[0]){case "a":q[1]=x[1];q[2]=x[2];q[3]=x[3];q[4]=x[4];q[5]=x[5];q[6]=+(x[6]-l).toFixed(3);q[7]=+(x[7]-n).toFixed(3);break;case "v":q[1]=+(x[1]-n).toFixed(3);break;case "m":k=x[1],p=x[2];default:for(var c=1,t=x.length;c<t;c++)q[c]=+(x[c]-(c%2?l:
n)).toFixed(3)}else for(f[s]=[],"m"==x[0]&&(k=x[1]+l,p=x[2]+n),q=0,c=x.length;q<c;q++)f[s][q]=x[q];x=f[s].length;switch(f[s][0]){case "z":l=k;n=p;break;case "h":l+=+f[s][x-1];break;case "v":n+=+f[s][x-1];break;default:l+=+f[s][x-2],n+=+f[s][x-1]}}f.toString=z;e.rel=d(f);return f};a.path.toAbsolute=G;a.path.toCubic=I;a.path.map=function(a,b){if(!b)return a;var d,e,h,f,l,n,k;a=I(a);h=0;for(l=a.length;h<l;h++)for(k=a[h],f=1,n=k.length;f<n;f+=2)d=b.x(k[f],k[f+1]),e=b.y(k[f],k[f+1]),k[f]=d,k[f+1]=e;return a};
a.path.toString=z;a.path.clone=d});C.plugin(function(a,v,y,C){var A=Math.max,w=Math.min,z=function(a){this.items=[];this.bindings={};this.length=0;this.type="set";if(a)for(var f=0,n=a.length;f<n;f++)a[f]&&(this[this.items.length]=this.items[this.items.length]=a[f],this.length++)};v=z.prototype;v.push=function(){for(var a,f,n=0,k=arguments.length;n<k;n++)if(a=arguments[n])f=this.items.length,this[f]=this.items[f]=a,this.length++;return this};v.pop=function(){this.length&&delete this[this.length--];
return this.items.pop()};v.forEach=function(a,f){for(var n=0,k=this.items.length;n<k&&!1!==a.call(f,this.items[n],n);n++);return this};v.animate=function(d,f,n,u){"function"!=typeof n||n.length||(u=n,n=L.linear);d instanceof a._.Animation&&(u=d.callback,n=d.easing,f=n.dur,d=d.attr);var p=arguments;if(a.is(d,"array")&&a.is(p[p.length-1],"array"))var b=!0;var q,e=function(){q?this.b=q:q=this.b},l=0,r=u&&function(){l++==this.length&&u.call(this)};return this.forEach(function(a,l){k.once("snap.animcreated."+
a.id,e);b?p[l]&&a.animate.apply(a,p[l]):a.animate(d,f,n,r)})};v.remove=function(){for(;this.length;)this.pop().remove();return this};v.bind=function(a,f,k){var u={};if("function"==typeof f)this.bindings[a]=f;else{var p=k||a;this.bindings[a]=function(a){u[p]=a;f.attr(u)}}return this};v.attr=function(a){var f={},k;for(k in a)if(this.bindings[k])this.bindings[k](a[k]);else f[k]=a[k];a=0;for(k=this.items.length;a<k;a++)this.items[a].attr(f);return this};v.clear=function(){for(;this.length;)this.pop()};
v.splice=function(a,f,k){a=0>a?A(this.length+a,0):a;f=A(0,w(this.length-a,f));var u=[],p=[],b=[],q;for(q=2;q<arguments.length;q++)b.push(arguments[q]);for(q=0;q<f;q++)p.push(this[a+q]);for(;q<this.length-a;q++)u.push(this[a+q]);var e=b.length;for(q=0;q<e+u.length;q++)this.items[a+q]=this[a+q]=q<e?b[q]:u[q-e];for(q=this.items.length=this.length-=f-e;this[q];)delete this[q++];return new z(p)};v.exclude=function(a){for(var f=0,k=this.length;f<k;f++)if(this[f]==a)return this.splice(f,1),!0;return!1};
v.insertAfter=function(a){for(var f=this.items.length;f--;)this.items[f].insertAfter(a);return this};v.getBBox=function(){for(var a=[],f=[],k=[],u=[],p=this.items.length;p--;)if(!this.items[p].removed){var b=this.items[p].getBBox();a.push(b.x);f.push(b.y);k.push(b.x+b.width);u.push(b.y+b.height)}a=w.apply(0,a);f=w.apply(0,f);k=A.apply(0,k);u=A.apply(0,u);return{x:a,y:f,x2:k,y2:u,width:k-a,height:u-f,cx:a+(k-a)/2,cy:f+(u-f)/2}};v.clone=function(a){a=new z;for(var f=0,k=this.items.length;f<k;f++)a.push(this.items[f].clone());
return a};v.toString=function(){return"Snap\u2018s set"};v.type="set";a.set=function(){var a=new z;arguments.length&&a.push.apply(a,Array.prototype.slice.call(arguments,0));return a}});C.plugin(function(a,v,y,C){function A(a){var b=a[0];switch(b.toLowerCase()){case "t":return[b,0,0];case "m":return[b,1,0,0,1,0,0];case "r":return 4==a.length?[b,0,a[2],a[3] ]:[b,0];case "s":return 5==a.length?[b,1,1,a[3],a[4] ]:3==a.length?[b,1,1]:[b,1]}}function w(b,d,f){d=q(d).replace(/\.{3}|\u2026/g,b);b=a.parseTransformString(b)||
[];d=a.parseTransformString(d)||[];for(var k=Math.max(b.length,d.length),p=[],v=[],h=0,w,z,y,I;h<k;h++){y=b[h]||A(d[h]);I=d[h]||A(y);if(y[0]!=I[0]||"r"==y[0].toLowerCase()&&(y[2]!=I[2]||y[3]!=I[3])||"s"==y[0].toLowerCase()&&(y[3]!=I[3]||y[4]!=I[4])){b=a._.transform2matrix(b,f());d=a._.transform2matrix(d,f());p=[["m",b.a,b.b,b.c,b.d,b.e,b.f] ];v=[["m",d.a,d.b,d.c,d.d,d.e,d.f] ];break}p[h]=[];v[h]=[];w=0;for(z=Math.max(y.length,I.length);w<z;w++)w in y&&(p[h][w]=y[w]),w in I&&(v[h][w]=I[w])}return{from:u(p),
to:u(v),f:n(p)}}function z(a){return a}function d(a){return function(b){return+b.toFixed(3)+a}}function f(b){return a.rgb(b[0],b[1],b[2])}function n(a){var b=0,d,f,k,n,h,p,q=[];d=0;for(f=a.length;d<f;d++){h="[";p=['"'+a[d][0]+'"'];k=1;for(n=a[d].length;k<n;k++)p[k]="val["+b++ +"]";h+=p+"]";q[d]=h}return Function("val","return Snap.path.toString.call(["+q+"])")}function u(a){for(var b=[],d=0,f=a.length;d<f;d++)for(var k=1,n=a[d].length;k<n;k++)b.push(a[d][k]);return b}var p={},b=/[a-z]+$/i,q=String;
p.stroke=p.fill="colour";v.prototype.equal=function(a,b){return k("snap.util.equal",this,a,b).firstDefined()};k.on("snap.util.equal",function(e,k){var r,s;r=q(this.attr(e)||"");var x=this;if(r==+r&&k==+k)return{from:+r,to:+k,f:z};if("colour"==p[e])return r=a.color(r),s=a.color(k),{from:[r.r,r.g,r.b,r.opacity],to:[s.r,s.g,s.b,s.opacity],f:f};if("transform"==e||"gradientTransform"==e||"patternTransform"==e)return k instanceof a.Matrix&&(k=k.toTransformString()),a._.rgTransform.test(k)||(k=a._.svgTransform2string(k)),
w(r,k,function(){return x.getBBox(1)});if("d"==e||"path"==e)return r=a.path.toCubic(r,k),{from:u(r[0]),to:u(r[1]),f:n(r[0])};if("points"==e)return r=q(r).split(a._.separator),s=q(k).split(a._.separator),{from:r,to:s,f:function(a){return a}};aUnit=r.match(b);s=q(k).match(b);return aUnit&&aUnit==s?{from:parseFloat(r),to:parseFloat(k),f:d(aUnit)}:{from:this.asPX(e),to:this.asPX(e,k),f:z}})});C.plugin(function(a,v,y,C){var A=v.prototype,w="createTouch"in C.doc;v="click dblclick mousedown mousemove mouseout mouseover mouseup touchstart touchmove touchend touchcancel".split(" ");
var z={mousedown:"touchstart",mousemove:"touchmove",mouseup:"touchend"},d=function(a,b){var d="y"==a?"scrollTop":"scrollLeft",e=b&&b.node?b.node.ownerDocument:C.doc;return e[d in e.documentElement?"documentElement":"body"][d]},f=function(){this.returnValue=!1},n=function(){return this.originalEvent.preventDefault()},u=function(){this.cancelBubble=!0},p=function(){return this.originalEvent.stopPropagation()},b=function(){if(C.doc.addEventListener)return function(a,b,e,f){var k=w&&z[b]?z[b]:b,l=function(k){var l=
d("y",f),q=d("x",f);if(w&&z.hasOwnProperty(b))for(var r=0,u=k.targetTouches&&k.targetTouches.length;r<u;r++)if(k.targetTouches[r].target==a||a.contains(k.targetTouches[r].target)){u=k;k=k.targetTouches[r];k.originalEvent=u;k.preventDefault=n;k.stopPropagation=p;break}return e.call(f,k,k.clientX+q,k.clientY+l)};b!==k&&a.addEventListener(b,l,!1);a.addEventListener(k,l,!1);return function(){b!==k&&a.removeEventListener(b,l,!1);a.removeEventListener(k,l,!1);return!0}};if(C.doc.attachEvent)return function(a,
b,e,h){var k=function(a){a=a||h.node.ownerDocument.window.event;var b=d("y",h),k=d("x",h),k=a.clientX+k,b=a.clientY+b;a.preventDefault=a.preventDefault||f;a.stopPropagation=a.stopPropagation||u;return e.call(h,a,k,b)};a.attachEvent("on"+b,k);return function(){a.detachEvent("on"+b,k);return!0}}}(),q=[],e=function(a){for(var b=a.clientX,e=a.clientY,f=d("y"),l=d("x"),n,p=q.length;p--;){n=q[p];if(w)for(var r=a.touches&&a.touches.length,u;r--;){if(u=a.touches[r],u.identifier==n.el._drag.id||n.el.node.contains(u.target)){b=
u.clientX;e=u.clientY;(a.originalEvent?a.originalEvent:a).preventDefault();break}}else a.preventDefault();b+=l;e+=f;k("snap.drag.move."+n.el.id,n.move_scope||n.el,b-n.el._drag.x,e-n.el._drag.y,b,e,a)}},l=function(b){a.unmousemove(e).unmouseup(l);for(var d=q.length,f;d--;)f=q[d],f.el._drag={},k("snap.drag.end."+f.el.id,f.end_scope||f.start_scope||f.move_scope||f.el,b);q=[]};for(y=v.length;y--;)(function(d){a[d]=A[d]=function(e,f){a.is(e,"function")&&(this.events=this.events||[],this.events.push({name:d,
f:e,unbind:b(this.node||document,d,e,f||this)}));return this};a["un"+d]=A["un"+d]=function(a){for(var b=this.events||[],e=b.length;e--;)if(b[e].name==d&&(b[e].f==a||!a)){b[e].unbind();b.splice(e,1);!b.length&&delete this.events;break}return this}})(v[y]);A.hover=function(a,b,d,e){return this.mouseover(a,d).mouseout(b,e||d)};A.unhover=function(a,b){return this.unmouseover(a).unmouseout(b)};var r=[];A.drag=function(b,d,f,h,n,p){function u(r,v,w){(r.originalEvent||r).preventDefault();this._drag.x=v;
this._drag.y=w;this._drag.id=r.identifier;!q.length&&a.mousemove(e).mouseup(l);q.push({el:this,move_scope:h,start_scope:n,end_scope:p});d&&k.on("snap.drag.start."+this.id,d);b&&k.on("snap.drag.move."+this.id,b);f&&k.on("snap.drag.end."+this.id,f);k("snap.drag.start."+this.id,n||h||this,v,w,r)}if(!arguments.length){var v;return this.drag(function(a,b){this.attr({transform:v+(v?"T":"t")+[a,b]})},function(){v=this.transform().local})}this._drag={};r.push({el:this,start:u});this.mousedown(u);return this};
A.undrag=function(){for(var b=r.length;b--;)r[b].el==this&&(this.unmousedown(r[b].start),r.splice(b,1),k.unbind("snap.drag.*."+this.id));!r.length&&a.unmousemove(e).unmouseup(l);return this}});C.plugin(function(a,v,y,C){y=y.prototype;var A=/^\s*url\((.+)\)/,w=String,z=a._.$;a.filter={};y.filter=function(d){var f=this;"svg"!=f.type&&(f=f.paper);d=a.parse(w(d));var k=a._.id(),u=z("filter");z(u,{id:k,filterUnits:"userSpaceOnUse"});u.appendChild(d.node);f.defs.appendChild(u);return new v(u)};k.on("snap.util.getattr.filter",
function(){k.stop();var d=z(this.node,"filter");if(d)return(d=w(d).match(A))&&a.select(d[1])});k.on("snap.util.attr.filter",function(d){if(d instanceof v&&"filter"==d.type){k.stop();var f=d.node.id;f||(z(d.node,{id:d.id}),f=d.id);z(this.node,{filter:a.url(f)})}d&&"none"!=d||(k.stop(),this.node.removeAttribute("filter"))});a.filter.blur=function(d,f){null==d&&(d=2);return a.format('<feGaussianBlur stdDeviation="{def}"/>',{def:null==f?d:[d,f]})};a.filter.blur.toString=function(){return this()};a.filter.shadow=
function(d,f,k,u,p){"string"==typeof k&&(p=u=k,k=4);"string"!=typeof u&&(p=u,u="#000");null==k&&(k=4);null==p&&(p=1);null==d&&(d=0,f=2);null==f&&(f=d);u=a.color(u||"#000");return a.format('<feGaussianBlur in="SourceAlpha" stdDeviation="{blur}"/><feOffset dx="{dx}" dy="{dy}" result="offsetblur"/><feFlood flood-color="{color}"/><feComposite in2="offsetblur" operator="in"/><feComponentTransfer><feFuncA type="linear" slope="{opacity}"/></feComponentTransfer><feMerge><feMergeNode/><feMergeNode in="SourceGraphic"/></feMerge>',
{color:u,dx:d,dy:f,blur:k,opacity:p})};a.filter.shadow.toString=function(){return this()};a.filter.grayscale=function(d){null==d&&(d=1);return a.format('<feColorMatrix type="matrix" values="{a} {b} {c} 0 0 {d} {e} {f} 0 0 {g} {b} {h} 0 0 0 0 0 1 0"/>',{a:0.2126+0.7874*(1-d),b:0.7152-0.7152*(1-d),c:0.0722-0.0722*(1-d),d:0.2126-0.2126*(1-d),e:0.7152+0.2848*(1-d),f:0.0722-0.0722*(1-d),g:0.2126-0.2126*(1-d),h:0.0722+0.9278*(1-d)})};a.filter.grayscale.toString=function(){return this()};a.filter.sepia=
function(d){null==d&&(d=1);return a.format('<feColorMatrix type="matrix" values="{a} {b} {c} 0 0 {d} {e} {f} 0 0 {g} {h} {i} 0 0 0 0 0 1 0"/>',{a:0.393+0.607*(1-d),b:0.769-0.769*(1-d),c:0.189-0.189*(1-d),d:0.349-0.349*(1-d),e:0.686+0.314*(1-d),f:0.168-0.168*(1-d),g:0.272-0.272*(1-d),h:0.534-0.534*(1-d),i:0.131+0.869*(1-d)})};a.filter.sepia.toString=function(){return this()};a.filter.saturate=function(d){null==d&&(d=1);return a.format('<feColorMatrix type="saturate" values="{amount}"/>',{amount:1-
d})};a.filter.saturate.toString=function(){return this()};a.filter.hueRotate=function(d){return a.format('<feColorMatrix type="hueRotate" values="{angle}"/>',{angle:d||0})};a.filter.hueRotate.toString=function(){return this()};a.filter.invert=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="table" tableValues="{amount} {amount2}"/><feFuncG type="table" tableValues="{amount} {amount2}"/><feFuncB type="table" tableValues="{amount} {amount2}"/></feComponentTransfer>',{amount:d,
amount2:1-d})};a.filter.invert.toString=function(){return this()};a.filter.brightness=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="linear" slope="{amount}"/><feFuncG type="linear" slope="{amount}"/><feFuncB type="linear" slope="{amount}"/></feComponentTransfer>',{amount:d})};a.filter.brightness.toString=function(){return this()};a.filter.contrast=function(d){null==d&&(d=1);return a.format('<feComponentTransfer><feFuncR type="linear" slope="{amount}" intercept="{amount2}"/><feFuncG type="linear" slope="{amount}" intercept="{amount2}"/><feFuncB type="linear" slope="{amount}" intercept="{amount2}"/></feComponentTransfer>',
{amount:d,amount2:0.5-d/2})};a.filter.contrast.toString=function(){return this()}});return C});

]]> </script>
<script> <![CDATA[

(function (glob, factory) {
    // AMD support
    if (typeof define === "function" && define.amd) {
        // Define as an anonymous module
        define("Gadfly", ["Snap.svg"], function (Snap) {
            return factory(Snap);
        });
    } else {
        // Browser globals (glob is window)
        // Snap adds itself to window
        glob.Gadfly = factory(glob.Snap);
    }
}(this, function (Snap) {

var Gadfly = {};

// Get an x/y coordinate value in pixels
var xPX = function(fig, x) {
    var client_box = fig.node.getBoundingClientRect();
    return x * fig.node.viewBox.baseVal.width / client_box.width;
};

var yPX = function(fig, y) {
    var client_box = fig.node.getBoundingClientRect();
    return y * fig.node.viewBox.baseVal.height / client_box.height;
};


Snap.plugin(function (Snap, Element, Paper, global) {
    // Traverse upwards from a snap element to find and return the first
    // note with the "plotroot" class.
    Element.prototype.plotroot = function () {
        var element = this;
        while (!element.hasClass("plotroot") && element.parent() != null) {
            element = element.parent();
        }
        return element;
    };

    Element.prototype.svgroot = function () {
        var element = this;
        while (element.node.nodeName != "svg" && element.parent() != null) {
            element = element.parent();
        }
        return element;
    };

    Element.prototype.plotbounds = function () {
        var root = this.plotroot()
        var bbox = root.select(".guide.background").node.getBBox();
        return {
            x0: bbox.x,
            x1: bbox.x + bbox.width,
            y0: bbox.y,
            y1: bbox.y + bbox.height
        };
    };

    Element.prototype.plotcenter = function () {
        var root = this.plotroot()
        var bbox = root.select(".guide.background").node.getBBox();
        return {
            x: bbox.x + bbox.width / 2,
            y: bbox.y + bbox.height / 2
        };
    };

    // Emulate IE style mouseenter/mouseleave events, since Microsoft always
    // does everything right.
    // See: http://www.dynamic-tools.net/toolbox/isMouseLeaveOrEnter/
    var events = ["mouseenter", "mouseleave"];

    for (i in events) {
        (function (event_name) {
            var event_name = events[i];
            Element.prototype[event_name] = function (fn, scope) {
                if (Snap.is(fn, "function")) {
                    var fn2 = function (event) {
                        if (event.type != "mouseover" && event.type != "mouseout") {
                            return;
                        }

                        var reltg = event.relatedTarget ? event.relatedTarget :
                            event.type == "mouseout" ? event.toElement : event.fromElement;
                        while (reltg && reltg != this.node) reltg = reltg.parentNode;

                        if (reltg != this.node) {
                            return fn.apply(this, event);
                        }
                    };

                    if (event_name == "mouseenter") {
                        this.mouseover(fn2, scope);
                    } else {
                        this.mouseout(fn2, scope);
                    }
                }
                return this;
            };
        })(events[i]);
    }


    Element.prototype.mousewheel = function (fn, scope) {
        if (Snap.is(fn, "function")) {
            var el = this;
            var fn2 = function (event) {
                fn.apply(el, [event]);
            };
        }

        this.node.addEventListener(
            /Firefox/i.test(navigator.userAgent) ? "DOMMouseScroll" : "mousewheel",
            fn2);

        return this;
    };


    // Snap's attr function can be too slow for things like panning/zooming.
    // This is a function to directly update element attributes without going
    // through eve.
    Element.prototype.attribute = function(key, val) {
        if (val === undefined) {
            return this.node.getAttribute(key);
        } else {
            this.node.setAttribute(key, val);
            return this;
        }
    };

    Element.prototype.init_gadfly = function() {
        this.mouseenter(Gadfly.plot_mouseover)
            .mouseleave(Gadfly.plot_mouseout)
            .dblclick(Gadfly.plot_dblclick)
            .mousewheel(Gadfly.guide_background_scroll)
            .drag(Gadfly.guide_background_drag_onmove,
                  Gadfly.guide_background_drag_onstart,
                  Gadfly.guide_background_drag_onend);
        this.mouseenter(function (event) {
            init_pan_zoom(this.plotroot());
        });
        return this;
    };
});


// When the plot is moused over, emphasize the grid lines.
Gadfly.plot_mouseover = function(event) {
    var root = this.plotroot();

    var keyboard_zoom = function(event) {
        if (event.which == 187) { // plus
            increase_zoom_by_position(root, 0.1, true);
        } else if (event.which == 189) { // minus
            increase_zoom_by_position(root, -0.1, true);
        }
    };
    root.data("keyboard_zoom", keyboard_zoom);
    window.addEventListener("keyup", keyboard_zoom);

    var xgridlines = root.select(".xgridlines"),
        ygridlines = root.select(".ygridlines");

    xgridlines.data("unfocused_strokedash",
                    xgridlines.attribute("stroke-dasharray").replace(/(\d)(,|$)/g, "$1mm$2"));
    ygridlines.data("unfocused_strokedash",
                    ygridlines.attribute("stroke-dasharray").replace(/(\d)(,|$)/g, "$1mm$2"));

    // emphasize grid lines
    var destcolor = root.data("focused_xgrid_color");
    xgridlines.attribute("stroke-dasharray", "none")
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    destcolor = root.data("focused_ygrid_color");
    ygridlines.attribute("stroke-dasharray", "none")
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    // reveal zoom slider
    root.select(".zoomslider")
        .animate({opacity: 1.0}, 250);
};

// Reset pan and zoom on double click
Gadfly.plot_dblclick = function(event) {
  set_plot_pan_zoom(this.plotroot(), 0.0, 0.0, 1.0);
};

// Unemphasize grid lines on mouse out.
Gadfly.plot_mouseout = function(event) {
    var root = this.plotroot();

    window.removeEventListener("keyup", root.data("keyboard_zoom"));
    root.data("keyboard_zoom", undefined);

    var xgridlines = root.select(".xgridlines"),
        ygridlines = root.select(".ygridlines");

    var destcolor = root.data("unfocused_xgrid_color");

    xgridlines.attribute("stroke-dasharray", xgridlines.data("unfocused_strokedash"))
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    destcolor = root.data("unfocused_ygrid_color");
    ygridlines.attribute("stroke-dasharray", ygridlines.data("unfocused_strokedash"))
              .selectAll("path")
              .animate({stroke: destcolor}, 250);

    // hide zoom slider
    root.select(".zoomslider")
        .animate({opacity: 0.0}, 250);
};


var set_geometry_transform = function(root, tx, ty, scale) {
    var xscalable = root.hasClass("xscalable"),
        yscalable = root.hasClass("yscalable");

    var old_scale = root.data("scale");

    var xscale = xscalable ? scale : 1.0,
        yscale = yscalable ? scale : 1.0;

    tx = xscalable ? tx : 0.0;
    ty = yscalable ? ty : 0.0;

    var t = new Snap.Matrix().translate(tx, ty).scale(xscale, yscale);

    root.selectAll(".geometry, image")
        .forEach(function (element, i) {
            element.transform(t);
        });

    bounds = root.plotbounds();

    if (yscalable) {
        var xfixed_t = new Snap.Matrix().translate(0, ty).scale(1.0, yscale);
        root.selectAll(".xfixed")
            .forEach(function (element, i) {
                element.transform(xfixed_t);
            });

        root.select(".ylabels")
            .transform(xfixed_t)
            .selectAll("text")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var cx = element.asPX("x"),
                        cy = element.asPX("y");
                    var st = element.data("static_transform");
                    unscale_t = new Snap.Matrix();
                    unscale_t.scale(1, 1/scale, cx, cy).add(st);
                    element.transform(unscale_t);

                    var y = cy * scale + ty;
                    element.attr("visibility",
                        bounds.y0 <= y && y <= bounds.y1 ? "visible" : "hidden");
                }
            });
    }

    if (xscalable) {
        var yfixed_t = new Snap.Matrix().translate(tx, 0).scale(xscale, 1.0);
        var xtrans = new Snap.Matrix().translate(tx, 0);
        root.selectAll(".yfixed")
            .forEach(function (element, i) {
                element.transform(yfixed_t);
            });

        root.select(".xlabels")
            .transform(yfixed_t)
            .selectAll("text")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var cx = element.asPX("x"),
                        cy = element.asPX("y");
                    var st = element.data("static_transform");
                    unscale_t = new Snap.Matrix();
                    unscale_t.scale(1/scale, 1, cx, cy).add(st);

                    element.transform(unscale_t);

                    var x = cx * scale + tx;
                    element.attr("visibility",
                        bounds.x0 <= x && x <= bounds.x1 ? "visible" : "hidden");
                    }
            });
    }

    // we must unscale anything that is scale invariance: widths, raiduses, etc.
    var size_attribs = ["font-size"];
    var unscaled_selection = ".geometry, .geometry *";
    if (xscalable) {
        size_attribs.push("rx");
        unscaled_selection += ", .xgridlines";
    }
    if (yscalable) {
        size_attribs.push("ry");
        unscaled_selection += ", .ygridlines";
    }

    root.selectAll(unscaled_selection)
        .forEach(function (element, i) {
            // circle need special help
            if (element.node.nodeName == "circle") {
                var cx = element.attribute("cx"),
                    cy = element.attribute("cy");
                unscale_t = new Snap.Matrix().scale(1/xscale, 1/yscale,
                                                        cx, cy);
                element.transform(unscale_t);
                return;
            }

            for (i in size_attribs) {
                var key = size_attribs[i];
                var val = parseFloat(element.attribute(key));
                if (val !== undefined && val != 0 && !isNaN(val)) {
                    element.attribute(key, val * old_scale / scale);
                }
            }
        });
};


// Find the most appropriate tick scale and update label visibility.
var update_tickscale = function(root, scale, axis) {
    if (!root.hasClass(axis + "scalable")) return;

    var tickscales = root.data(axis + "tickscales");
    var best_tickscale = 1.0;
    var best_tickscale_dist = Infinity;
    for (tickscale in tickscales) {
        var dist = Math.abs(Math.log(tickscale) - Math.log(scale));
        if (dist < best_tickscale_dist) {
            best_tickscale_dist = dist;
            best_tickscale = tickscale;
        }
    }

    if (best_tickscale != root.data(axis + "tickscale")) {
        root.data(axis + "tickscale", best_tickscale);
        var mark_inscale_gridlines = function (element, i) {
            var inscale = element.attr("gadfly:scale") == best_tickscale;
            element.attribute("gadfly:inscale", inscale);
            element.attr("visibility", inscale ? "visible" : "hidden");
        };

        var mark_inscale_labels = function (element, i) {
            var inscale = element.attr("gadfly:scale") == best_tickscale;
            element.attribute("gadfly:inscale", inscale);
            element.attr("visibility", inscale ? "visible" : "hidden");
        };

        root.select("." + axis + "gridlines").selectAll("path").forEach(mark_inscale_gridlines);
        root.select("." + axis + "labels").selectAll("text").forEach(mark_inscale_labels);
    }
};


var set_plot_pan_zoom = function(root, tx, ty, scale) {
    var old_scale = root.data("scale");
    var bounds = root.plotbounds();

    var width = bounds.x1 - bounds.x0,
        height = bounds.y1 - bounds.y0;

    // compute the viewport derived from tx, ty, and scale
    var x_min = -width * scale - (scale * width - width),
        x_max = width * scale,
        y_min = -height * scale - (scale * height - height),
        y_max = height * scale;

    var x0 = bounds.x0 - scale * bounds.x0,
        y0 = bounds.y0 - scale * bounds.y0;

    var tx = Math.max(Math.min(tx - x0, x_max), x_min),
        ty = Math.max(Math.min(ty - y0, y_max), y_min);

    tx += x0;
    ty += y0;

    // when the scale change, we may need to alter which set of
    // ticks is being displayed
    if (scale != old_scale) {
        update_tickscale(root, scale, "x");
        update_tickscale(root, scale, "y");
    }

    set_geometry_transform(root, tx, ty, scale);

    root.data("scale", scale);
    root.data("tx", tx);
    root.data("ty", ty);
};


var scale_centered_translation = function(root, scale) {
    var bounds = root.plotbounds();

    var width = bounds.x1 - bounds.x0,
        height = bounds.y1 - bounds.y0;

    var tx0 = root.data("tx"),
        ty0 = root.data("ty");

    var scale0 = root.data("scale");

    // how off from center the current view is
    var xoff = tx0 - (bounds.x0 * (1 - scale0) + (width * (1 - scale0)) / 2),
        yoff = ty0 - (bounds.y0 * (1 - scale0) + (height * (1 - scale0)) / 2);

    // rescale offsets
    xoff = xoff * scale / scale0;
    yoff = yoff * scale / scale0;

    // adjust for the panel position being scaled
    var x_edge_adjust = bounds.x0 * (1 - scale),
        y_edge_adjust = bounds.y0 * (1 - scale);

    return {
        x: xoff + x_edge_adjust + (width - width * scale) / 2,
        y: yoff + y_edge_adjust + (height - height * scale) / 2
    };
};


// Initialize data for panning zooming if it isn't already.
var init_pan_zoom = function(root) {
    if (root.data("zoompan-ready")) {
        return;
    }

    // The non-scaling-stroke trick. Rather than try to correct for the
    // stroke-width when zooming, we force it to a fixed value.
    var px_per_mm = root.node.getCTM().a;

    // Drag events report deltas in pixels, which we'd like to convert to
    // millimeters.
    root.data("px_per_mm", px_per_mm);

    root.selectAll("path")
        .forEach(function (element, i) {
        sw = element.asPX("stroke-width") * px_per_mm;
        if (sw > 0) {
            element.attribute("stroke-width", sw);
            element.attribute("vector-effect", "non-scaling-stroke");
        }
    });

    // Store ticks labels original tranformation
    root.selectAll(".xlabels > text, .ylabels > text")
        .forEach(function (element, i) {
            var lm = element.transform().localMatrix;
            element.data("static_transform",
                new Snap.Matrix(lm.a, lm.b, lm.c, lm.d, lm.e, lm.f));
        });

    var xgridlines = root.select(".xgridlines");
    var ygridlines = root.select(".ygridlines");
    var xlabels = root.select(".xlabels");
    var ylabels = root.select(".ylabels");

    if (root.data("tx") === undefined) root.data("tx", 0);
    if (root.data("ty") === undefined) root.data("ty", 0);
    if (root.data("scale") === undefined) root.data("scale", 1.0);
    if (root.data("xtickscales") === undefined) {

        // index all the tick scales that are listed
        var xtickscales = {};
        var ytickscales = {};
        var add_x_tick_scales = function (element, i) {
            xtickscales[element.attribute("gadfly:scale")] = true;
        };
        var add_y_tick_scales = function (element, i) {
            ytickscales[element.attribute("gadfly:scale")] = true;
        };

        if (xgridlines) xgridlines.selectAll("path").forEach(add_x_tick_scales);
        if (ygridlines) ygridlines.selectAll("path").forEach(add_y_tick_scales);
        if (xlabels) xlabels.selectAll("text").forEach(add_x_tick_scales);
        if (ylabels) ylabels.selectAll("text").forEach(add_y_tick_scales);

        root.data("xtickscales", xtickscales);
        root.data("ytickscales", ytickscales);
        root.data("xtickscale", 1.0);
    }

    var min_scale = 1.0, max_scale = 1.0;
    for (scale in xtickscales) {
        min_scale = Math.min(min_scale, scale);
        max_scale = Math.max(max_scale, scale);
    }
    for (scale in ytickscales) {
        min_scale = Math.min(min_scale, scale);
        max_scale = Math.max(max_scale, scale);
    }
    root.data("min_scale", min_scale);
    root.data("max_scale", max_scale);

    // store the original positions of labels
    if (xlabels) {
        xlabels.selectAll("text")
               .forEach(function (element, i) {
                   element.data("x", element.asPX("x"));
               });
    }

    if (ylabels) {
        ylabels.selectAll("text")
               .forEach(function (element, i) {
                   element.data("y", element.asPX("y"));
               });
    }

    // mark grid lines and ticks as in or out of scale.
    var mark_inscale = function (element, i) {
        element.attribute("gadfly:inscale", element.attribute("gadfly:scale") == 1.0);
    };

    if (xgridlines) xgridlines.selectAll("path").forEach(mark_inscale);
    if (ygridlines) ygridlines.selectAll("path").forEach(mark_inscale);
    if (xlabels) xlabels.selectAll("text").forEach(mark_inscale);
    if (ylabels) ylabels.selectAll("text").forEach(mark_inscale);

    // figure out the upper ond lower bounds on panning using the maximum
    // and minum grid lines
    var bounds = root.plotbounds();
    var pan_bounds = {
        x0: 0.0,
        y0: 0.0,
        x1: 0.0,
        y1: 0.0
    };

    if (xgridlines) {
        xgridlines
            .selectAll("path")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var bbox = element.node.getBBox();
                    if (bounds.x1 - bbox.x < pan_bounds.x0) {
                        pan_bounds.x0 = bounds.x1 - bbox.x;
                    }
                    if (bounds.x0 - bbox.x > pan_bounds.x1) {
                        pan_bounds.x1 = bounds.x0 - bbox.x;
                    }
                    element.attr("visibility", "visible");
                }
            });
    }

    if (ygridlines) {
        ygridlines
            .selectAll("path")
            .forEach(function (element, i) {
                if (element.attribute("gadfly:inscale") == "true") {
                    var bbox = element.node.getBBox();
                    if (bounds.y1 - bbox.y < pan_bounds.y0) {
                        pan_bounds.y0 = bounds.y1 - bbox.y;
                    }
                    if (bounds.y0 - bbox.y > pan_bounds.y1) {
                        pan_bounds.y1 = bounds.y0 - bbox.y;
                    }
                    element.attr("visibility", "visible");
                }
            });
    }

    // nudge these values a little
    pan_bounds.x0 -= 5;
    pan_bounds.x1 += 5;
    pan_bounds.y0 -= 5;
    pan_bounds.y1 += 5;
    root.data("pan_bounds", pan_bounds);

    root.data("zoompan-ready", true)
};


// drag actions, i.e. zooming and panning
var pan_action = {
    start: function(root, x, y, event) {
        root.data("dx", 0);
        root.data("dy", 0);
        root.data("tx0", root.data("tx"));
        root.data("ty0", root.data("ty"));
    },
    update: function(root, dx, dy, x, y, event) {
        var px_per_mm = root.data("px_per_mm");
        dx /= px_per_mm;
        dy /= px_per_mm;

        var tx0 = root.data("tx"),
            ty0 = root.data("ty");

        var dx0 = root.data("dx"),
            dy0 = root.data("dy");

        root.data("dx", dx);
        root.data("dy", dy);

        dx = dx - dx0;
        dy = dy - dy0;

        var tx = tx0 + dx,
            ty = ty0 + dy;

        set_plot_pan_zoom(root, tx, ty, root.data("scale"));
    },
    end: function(root, event) {

    },
    cancel: function(root) {
        set_plot_pan_zoom(root, root.data("tx0"), root.data("ty0"), root.data("scale"));
    }
};

var zoom_box;
var zoom_action = {
    start: function(root, x, y, event) {
        var bounds = root.plotbounds();
        var width = bounds.x1 - bounds.x0,
            height = bounds.y1 - bounds.y0;
        var ratio = width / height;
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var px_per_mm = root.data("px_per_mm");
        x = xscalable ? x / px_per_mm : bounds.x0;
        y = yscalable ? y / px_per_mm : bounds.y0;
        var w = xscalable ? 0 : width;
        var h = yscalable ? 0 : height;
        zoom_box = root.rect(x, y, w, h).attr({
            "fill": "#000",
            "opacity": 0.25
        });
        zoom_box.data("ratio", ratio);
    },
    update: function(root, dx, dy, x, y, event) {
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var px_per_mm = root.data("px_per_mm");
        var bounds = root.plotbounds();
        if (yscalable) {
            y /= px_per_mm;
            y = Math.max(bounds.y0, y);
            y = Math.min(bounds.y1, y);
        } else {
            y = bounds.y1;
        }
        if (xscalable) {
            x /= px_per_mm;
            x = Math.max(bounds.x0, x);
            x = Math.min(bounds.x1, x);
        } else {
            x = bounds.x1;
        }

        dx = x - zoom_box.attr("x");
        dy = y - zoom_box.attr("y");
        if (xscalable && yscalable) {
            var ratio = zoom_box.data("ratio");
            var width = Math.min(Math.abs(dx), ratio * Math.abs(dy));
            var height = Math.min(Math.abs(dy), Math.abs(dx) / ratio);
            dx = width * dx / Math.abs(dx);
            dy = height * dy / Math.abs(dy);
        }
        var xoffset = 0,
            yoffset = 0;
        if (dx < 0) {
            xoffset = dx;
            dx = -1 * dx;
        }
        if (dy < 0) {
            yoffset = dy;
            dy = -1 * dy;
        }
        if (isNaN(dy)) {
            dy = 0.0;
        }
        if (isNaN(dx)) {
            dx = 0.0;
        }
        zoom_box.transform("T" + xoffset + "," + yoffset);
        zoom_box.attr("width", dx);
        zoom_box.attr("height", dy);
    },
    end: function(root, event) {
        var xscalable = root.hasClass("xscalable"),
            yscalable = root.hasClass("yscalable");
        var zoom_bounds = zoom_box.getBBox();
        if (zoom_bounds.width * zoom_bounds.height <= 0) {
            return;
        }
        var plot_bounds = root.plotbounds();
        var zoom_factor = 1.0;
        if (yscalable) {
            zoom_factor = (plot_bounds.y1 - plot_bounds.y0) / zoom_bounds.height;
        } else {
            zoom_factor = (plot_bounds.x1 - plot_bounds.x0) / zoom_bounds.width;
        }
        var tx = (root.data("tx") - zoom_bounds.x) * zoom_factor + plot_bounds.x0,
            ty = (root.data("ty") - zoom_bounds.y) * zoom_factor + plot_bounds.y0;
        set_plot_pan_zoom(root, tx, ty, root.data("scale") * zoom_factor);
        zoom_box.remove();
    },
    cancel: function(root) {
        zoom_box.remove();
    }
};


Gadfly.guide_background_drag_onstart = function(x, y, event) {
    var root = this.plotroot();
    var scalable = root.hasClass("xscalable") || root.hasClass("yscalable");
    var zoomable = !event.altKey && !event.ctrlKey && event.shiftKey && scalable;
    var panable = !event.altKey && !event.ctrlKey && !event.shiftKey && scalable;
    var drag_action = zoomable ? zoom_action :
                      panable  ? pan_action :
                                 undefined;
    root.data("drag_action", drag_action);
    if (drag_action) {
        var cancel_drag_action = function(event) {
            if (event.which == 27) { // esc key
                drag_action.cancel(root);
                root.data("drag_action", undefined);
            }
        };
        window.addEventListener("keyup", cancel_drag_action);
        root.data("cancel_drag_action", cancel_drag_action);
        drag_action.start(root, x, y, event);
    }
};


Gadfly.guide_background_drag_onmove = function(dx, dy, x, y, event) {
    var root = this.plotroot();
    var drag_action = root.data("drag_action");
    if (drag_action) {
        drag_action.update(root, dx, dy, x, y, event);
    }
};


Gadfly.guide_background_drag_onend = function(event) {
    var root = this.plotroot();
    window.removeEventListener("keyup", root.data("cancel_drag_action"));
    root.data("cancel_drag_action", undefined);
    var drag_action = root.data("drag_action");
    if (drag_action) {
        drag_action.end(root, event);
    }
    root.data("drag_action", undefined);
};


Gadfly.guide_background_scroll = function(event) {
    if (event.shiftKey) {
        increase_zoom_by_position(this.plotroot(), 0.001 * event.wheelDelta);
        event.preventDefault();
    }
};


Gadfly.zoomslider_button_mouseover = function(event) {
    this.select(".button_logo")
         .animate({fill: this.data("mouseover_color")}, 100);
};


Gadfly.zoomslider_button_mouseout = function(event) {
     this.select(".button_logo")
         .animate({fill: this.data("mouseout_color")}, 100);
};


Gadfly.zoomslider_zoomout_click = function(event) {
    increase_zoom_by_position(this.plotroot(), -0.1, true);
};


Gadfly.zoomslider_zoomin_click = function(event) {
    increase_zoom_by_position(this.plotroot(), 0.1, true);
};


Gadfly.zoomslider_track_click = function(event) {
    // TODO
};


// Map slider position x to scale y using the function y = a*exp(b*x)+c.
// The constants a, b, and c are solved using the constraint that the function
// should go through the points (0; min_scale), (0.5; 1), and (1; max_scale).
var scale_from_slider_position = function(position, min_scale, max_scale) {
    var a = (1 - 2 * min_scale + min_scale * min_scale) / (min_scale + max_scale - 2),
        b = 2 * Math.log((max_scale - 1) / (1 - min_scale)),
        c = (min_scale * max_scale - 1) / (min_scale + max_scale - 2);
    return a * Math.exp(b * position) + c;
}

// inverse of scale_from_slider_position
var slider_position_from_scale = function(scale, min_scale, max_scale) {
    var a = (1 - 2 * min_scale + min_scale * min_scale) / (min_scale + max_scale - 2),
        b = 2 * Math.log((max_scale - 1) / (1 - min_scale)),
        c = (min_scale * max_scale - 1) / (min_scale + max_scale - 2);
    return 1 / b * Math.log((scale - c) / a);
}

var increase_zoom_by_position = function(root, delta_position, animate) {
    var scale = root.data("scale"),
        min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale");
    var position = slider_position_from_scale(scale, min_scale, max_scale);
    position += delta_position;
    scale = scale_from_slider_position(position, min_scale, max_scale);
    set_zoom(root, scale, animate);
}

var set_zoom = function(root, scale, animate) {
    var min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale"),
        old_scale = root.data("scale");
    var new_scale = Math.max(min_scale, Math.min(scale, max_scale));
    if (animate) {
        Snap.animate(
            old_scale,
            new_scale,
            function (new_scale) {
                update_plot_scale(root, new_scale);
            },
            200);
    } else {
        update_plot_scale(root, new_scale);
    }
}


var update_plot_scale = function(root, new_scale) {
    var trans = scale_centered_translation(root, new_scale);
    set_plot_pan_zoom(root, trans.x, trans.y, new_scale);

    root.selectAll(".zoomslider_thumb")
        .forEach(function (element, i) {
            var min_pos = element.data("min_pos"),
                max_pos = element.data("max_pos"),
                min_scale = root.data("min_scale"),
                max_scale = root.data("max_scale");
            var xmid = (min_pos + max_pos) / 2;
            var xpos = slider_position_from_scale(new_scale, min_scale, max_scale);
            element.transform(new Snap.Matrix().translate(
                Math.max(min_pos, Math.min(
                         max_pos, min_pos + (max_pos - min_pos) * xpos)) - xmid, 0));
    });
};


Gadfly.zoomslider_thumb_dragmove = function(dx, dy, x, y, event) {
    var root = this.plotroot();
    var min_pos = this.data("min_pos"),
        max_pos = this.data("max_pos"),
        min_scale = root.data("min_scale"),
        max_scale = root.data("max_scale"),
        old_scale = root.data("old_scale");

    var px_per_mm = root.data("px_per_mm");
    dx /= px_per_mm;
    dy /= px_per_mm;

    var xmid = (min_pos + max_pos) / 2;
    var xpos = slider_position_from_scale(old_scale, min_scale, max_scale) +
                   dx / (max_pos - min_pos);

    // compute the new scale
    var new_scale = scale_from_slider_position(xpos, min_scale, max_scale);
    new_scale = Math.min(max_scale, Math.max(min_scale, new_scale));

    update_plot_scale(root, new_scale);
    event.stopPropagation();
};


Gadfly.zoomslider_thumb_dragstart = function(x, y, event) {
    this.animate({fill: this.data("mouseover_color")}, 100);
    var root = this.plotroot();

    // keep track of what the scale was when we started dragging
    root.data("old_scale", root.data("scale"));
    event.stopPropagation();
};


Gadfly.zoomslider_thumb_dragend = function(event) {
    this.animate({fill: this.data("mouseout_color")}, 100);
    event.stopPropagation();
};


var toggle_color_class = function(root, color_class, ison) {
    var guides = root.selectAll(".guide." + color_class + ",.guide ." + color_class);
    var geoms = root.selectAll(".geometry." + color_class + ",.geometry ." + color_class);
    if (ison) {
        guides.animate({opacity: 0.5}, 250);
        geoms.animate({opacity: 0.0}, 250);
    } else {
        guides.animate({opacity: 1.0}, 250);
        geoms.animate({opacity: 1.0}, 250);
    }
};


Gadfly.colorkey_swatch_click = function(event) {
    var root = this.plotroot();
    var color_class = this.data("color_class");

    if (event.shiftKey) {
        root.selectAll(".colorkey text")
            .forEach(function (element) {
                var other_color_class = element.data("color_class");
                if (other_color_class != color_class) {
                    toggle_color_class(root, other_color_class,
                                       element.attr("opacity") == 1.0);
                }
            });
    } else {
        toggle_color_class(root, color_class, this.attr("opacity") == 1.0);
    }
};


return Gadfly;

}));


//@ sourceURL=gadfly.js

(function (glob, factory) {
    // AMD support
      if (typeof require === "function" && typeof define === "function" && define.amd) {
        require(["Snap.svg", "Gadfly"], function (Snap, Gadfly) {
            factory(Snap, Gadfly);
        });
      } else {
          factory(glob.Snap, glob.Gadfly);
      }
})(window, function (Snap, Gadfly) {
    var fig = Snap("#img-e512d6eb");
fig.select("#img-e512d6eb-5")
   .init_gadfly();
fig.select("#img-e512d6eb-7")
   .plotroot().data("unfocused_ygrid_color", "#D0D0E0")
;
fig.select("#img-e512d6eb-7")
   .plotroot().data("focused_ygrid_color", "#A0A0A0")
;
fig.select("#img-e512d6eb-8")
   .plotroot().data("unfocused_xgrid_color", "#D0D0E0")
;
fig.select("#img-e512d6eb-8")
   .plotroot().data("focused_xgrid_color", "#A0A0A0")
;
fig.select("#img-e512d6eb-12")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-e512d6eb-12")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-e512d6eb-12")
   .click(Gadfly.zoomslider_zoomin_click)
.mouseenter(Gadfly.zoomslider_button_mouseover)
.mouseleave(Gadfly.zoomslider_button_mouseout)
;
fig.select("#img-e512d6eb-14")
   .data("max_pos", 120.42)
;
fig.select("#img-e512d6eb-14")
   .data("min_pos", 103.42)
;
fig.select("#img-e512d6eb-14")
   .click(Gadfly.zoomslider_track_click);
fig.select("#img-e512d6eb-15")
   .data("max_pos", 120.42)
;
fig.select("#img-e512d6eb-15")
   .data("min_pos", 103.42)
;
fig.select("#img-e512d6eb-15")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-e512d6eb-15")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-e512d6eb-15")
   .drag(Gadfly.zoomslider_thumb_dragmove,
     Gadfly.zoomslider_thumb_dragstart,
     Gadfly.zoomslider_thumb_dragend)
;
fig.select("#img-e512d6eb-16")
   .data("mouseover_color", "#CD5C5C")
;
fig.select("#img-e512d6eb-16")
   .data("mouseout_color", "#6A6A6A")
;
fig.select("#img-e512d6eb-16")
   .click(Gadfly.zoomslider_zoomout_click)
.mouseenter(Gadfly.zoomslider_button_mouseover)
.mouseleave(Gadfly.zoomslider_button_mouseout)
;
    });
]]> </script>
</svg>




```julia

```
