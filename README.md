BIMserver
=========
1. GLTF2 loader:
var query = {includeAllFields: true}
loadBIM = function(roid, query, callback) {
	var self = this;
	self.bimServerApi.getSerializerByPluginClassName("org.bimserver.gltf.BinaryGltfSerializerPlugin2", function(serializer) {
		self.bimServerApi.call("ServiceInterface", "download", {
			roids: [roid],
			query: JSON.stringify(query),
			serializerOid: serializer.oid,
			sync: false
		}, function(topicId) {
			self.bimServerApi.registerProgressHandler(topicId, function(topicId, state) {
				if (state.title == "Done preparing") {
					var url = self.bimServerApi.generateRevisionDownloadUrl({
						zip:false,
						serializerOid: serializer.oid,
						topicId: topicId
					});
					console.log(url);
					callback(url);
				}
			});
		});
	});
} 



------------------------------*----------------------------------
The Building Information Model server (short: BIMserver) enables you to store and manage the information of a construction (or other building related) project. Data is stored in the open standard IFC. The BIMserver is not a fileserver, but it uses a model-driven architecture approach. This means that IFC data is stored in an underlying database. The main advantage of this approach is the ability to query, merge and filter the BIM-model and generate IFC files on the fly.

Thanks to it's multi-user support, multiple people can work on their own part of the model, while the complete model is updated on the fly. Other users can get notifications when the model (or a part of it) is updated. Furthermore the BIMserver will warn you when other users have changed something in the model while you were editing.

BIMserver is built for developers. We've got a great wiki on https://github.com/opensourceBIM/BIMserver/wiki and are very active supporting developers on https://github.com/opensourceBIM/BIMserver/issues 

See a full list of features on http://www.bimserver.org/ 

Licence: GNU Affero General Public License, version 3 (see http://www.gnu.org/licenses/agpl-3.0.html)
Beware: this project makes intensive use of several other projects with different licenses. Some plugins and libraries are published under a different license.
