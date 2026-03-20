Apply mapping from one geometry to the rest of the geometries in the dataset.
Train on field information from 3D file, 
## Infra
- Slipstream PR
- All alex work in Slipstream pr OR ___ (ask)
##### Images
- Come from NC, moved into own artifactory
- list of images in PE (plateng) repo
	- Location of images: [https://artifactory.teslamotors.com/ui/repos/tree/General/pe/neuralconcept](https://artifactory.teslamotors.com/ui/repos/tree/General/pe/neuralconcept "https://artifactory.teslamotors.com/ui/repos/tree/general/pe/neuralconcept")
		- Not needed to be in there, but how it was set up
	- Don't need to touch very often. When touched need to pull images from Artifactory.
- Only other thing is DB, using EVT stg and separate schema rn. Not sure what it is for? Users, roles, metadata, etc...
	- Only metadata, nothing to do with runs
	- Used by NC platform.
- k8s is everything we get from nc. nc.yaml is where we configure everything.

Fundamentally only thing that matters for each run is numbers at the end.
2 TB data volume. 100 GB dedicated to workspace files

