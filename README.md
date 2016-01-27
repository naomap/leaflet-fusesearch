<h1>leaflet-fusesearch</h1>

Search features in a GeoJSON layer using the lightweight JavaScript fuzzy search <a href="https://github.com/krisk/Fuse">Fuse.js</a>

<h2>Usage</h2>

First download Fuse.js from <a href="https://github.com/krisk/Fuse">this repo<a/> or 
from <a href="http://kiro.me/projects/fuse.html">Kiro's site</a>, and load it in your page before leaflet-fusesearch.js.<br/>
<br/>
Create the control L.Control.FuseSearch and add it to the Map.
<pre>
var searchCtrl = L.control.fuseSearch()
searchCtrl.addTo(map);
</pre>

Then load your GeoJSON layer and index the features, choosing the properties you want to index
(note you can pass either the FeatureCollection itself or its .features), e.g.
<pre>
searchCtrl.indexFeatures(jsonData, ['name', 'company', 'details']);
</pre>

Finally you need to bind each layer (marker) to the feature it is associated with, so that selecting an item in the result list opens up the matching popup. For instance :
<pre>
L.geoJson(data, {
    onEachFeature: function (feature, layer) {
        <strong>feature.layer = layer;</strong>
    }
});
</pre>

This is it !  By default the search control will appear on the top right corner of the map.
This opens the search pane on the same side where you can type in the search string.
The matching features are listed, with the indexed properties displayed. Clicking a feature
on the list opens up the matching pop-up on the map, provided one is associated with it.

<h2>Options</h2>

The FuseSearch control can be created with the following options :
<ul>
<li><code>position</code> : position of the control, the search pane shows on the matching side. Default <code>'topright'</code>.</li>
<li><code>title</code> : used for the control tooltip, default <code>'Search'</code></li>
<li><code>panelTitle</code> : title string to add on the top of the search panel, none by default</li>
<li><code>placeholder</code> : used for the input placeholder, default <code>'Search'</code></li>
<li><code>maxResultLength</code> : number of features displayed in the result list, default is null 
	and all features found by Fuse are displayed</li>
<li><code>showInvisibleFeatures</code> : display the matching features even if their layer is invisible, default true</li>
<li><code>showResultFct</code> : function to display a feature returned by the search, parameters are the
	feature and an HTML container. Here is an example :</li>
</ul>
<pre>
    showResultFct: function(feature, container) {
        props = feature.properties;
        var name = L.DomUtil.create('b', null, container);
        name.innerHTML = props.name;
        container.appendChild(L.DomUtil.create('br', null, container));
        container.appendChild(document.createTextNode(props.details));
    }
</pre>

In addition these options are directly passed to Fuse - more details on <a href="http://kiro.me/projects/fuse.html">Fuse.js</a> :
<ul>
<li><code>caseSensitive</code> : whether comparisons should be case sensitive, default is false</li>
<li><code>threshold</code> : a decimal value indicating at which point the match algorithm gives up. 
A threshold of 0.0 requires a perfect match, a threshold of 1.0 would match anything, default 0.5</li>
</ul>

<h2>Other functions</h2>
The function <code>refresh()</code> can be used to search again the indexed features. 
This is useful for instance when some features have been filtered out, and saves you from reindexing the features.

<h2>Example</h2>

I should really provide a simpler example, however for now have a look at my <a href="http://dev.cartocite.fr/CultureNantes/">demo site</a>.
