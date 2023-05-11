Just open the html and it should work
Based on https://docs.mapbox.com/mapbox-gl-js/example/mapbox-gl-draw/

To generate the html lines, use the following core query

-- Build one line like that for each selected area
-- <textarea id = '2' class='coord' rows='1' cols='40'>[[[29.3232,68.0652],[28.661,68.1989],...,[29.9708,67.698],[29.3232,68.0652]]]</textarea>


SELECT '<textarea id = ''' || row_number() OVER () || ''' class=''coord'' rows=''1'' cols=''40''>' || (ST_AsGeoJSON(polygon)::json->>'coordinates')::TEXT || '</textarea>' 
FROM areas 
LEFT JOIN geo_areas ON areas.id = geo_areas.area_id
WHERE areas.display_type = 'main_region'
AND SOURCE = 'manual'