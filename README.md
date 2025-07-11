# 🌾 Maui Agronomics Recommendation System (MARS)

MARS is a LangGraph-based agronomic assistant designed to help farmers make smarter decisions using imagery and weather data. It combines NDVI-based crop recommendations, pest detection powered by YOLOv11s (Small), and weather-aware insights (including agricultural evapotranspiration) into a single intelligent pipeline. The system uses Anthropic's Claude via LangChain to aggregate multimodal insights and output practical recommendations.

## 🧠 Key Features

- 🔍 **Pest Detection**: YOLOv11s model supports 102 pest classes. Supports all formats but pipeline only accepts jpeg and tiff/tif for now
- 🌱 **NDVI Pipeline**: Supports TIFF imagery, Calculates NDVI, CWR, and incorporates FAO Penman-Monteith evapotranspiration (ET₀) for precise irrigation insights.
- ☁️ **Weather-Aware Recommendations**: Factors in FAO Penman-Monteith evapotranspiration (ET₀) from open-meteo (https://open-meteo.com/en/docs)
- 🤖 **Maui Aggregation Agent**: Powered by ChatAnthropic via LangChain.
- ☁️ **Cloud Integration**: AWS S3 support for image storage and retrieval.

---

## 🛠 Tech Stack

- **Languages**: Python
- **Frameworks**: [LangChain](https://www.langchain.com/), [LangGraph](https://docs.langgraph.dev/), [LangSmith](https://smith.langchain.com/)
- **Vision Model**: YOLOv11s via [Ultralytics](https://github.com/ultralytics/ultralytics)

## Core Libraries
pydantic
requests
numpy
matplotlib
ultralytics
langchain-anthropic
langgraph
boto3
scikit-image
scipy
tifffile
Pillow
opencv-python


## ⚙️ Installation
Clone the repo

git clone https://github.com/your-username/mars.git
cd mars


Set up virtual environment

python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows


Install dependencies

pip install -r requirements.txt
Configure environment variables

Create a .env file at the root with the following:

OPENAI_API_KEY=your_openai_key
LANGSMITH_API_KEY=your_langsmith_key
LANGCHAIN_PROJECT=your_langchain_project_name
LANGSMITH_TRACING=true
AWS_ACCESS_KEY=your_aws_access_key
AWS_SECRET_KEY=your_aws_secret_key
AWS_REGION=your_aws_region
ANTHROPIC_API_KEY=your_anthropic_key


## 🚀 Usage

This project is designed to run as a LangGraph deployed pipeline, accessible via the LangChain SDK.

Query Maui with image inputs for pest detection (JPEG only) or NDVI-based recommendations + pest detection (TIFF).

Example

```python

#Initialize the client
from langgraph_sdk import get_client

client = get_client(url="https://ht-vacant-providence-60-b70b203b735b596eac574071f7251ad0.us.langgraph.app", api_key="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")
thread = await client.threads.create()
graph_name = "advisor"

user_input = {
    "user_id": "xyz",
    "coordinates": {
        "latitude": 37.7749,
        "longitude": -122.4194
    },
    "image_key": "ndvi_results/2025-04-19T18:37:44.241214_20180711_seq_50m_NC (1).tif"
}

#Simple invocation example
run = await client.runs.create(
   thread["thread_id"],
   graph_name,
   input=user_input,
)

# Wait until the run completes
await client.runs.join(thread["thread_id"], run["run_id"])

# Check the run status
print("Result structure: " await client.runs.get(thread["thread_id"], run["run_id"]))

Result structure: {'user_id': 'xyz',
 'yolo_result': {'status': 'Success',
  'class_labels': ['rice leaf roller',
   'rice leaf caterpillar',
   'paddy stem maggot',
   'asiatic rice borer',
   'yellow rice borer',
   'rice gall midge',
   'Rice Stemfly',
   'brown plant hopper',
   'white backed plant hopper',
   'small brown plant hopper',
   'rice water weevil',
   'rice leafhopper',
   'grain spreader thrips',
   'rice shell pest',
   'grub',
   'mole cricket',
   'wireworm',
   'white margined moth',
   'black cutworm',
   'large cutworm',
   'yellow cutworm',
   'red spider',
   'corn borer',
   'army worm',
   'aphids',
   'Potosiabre vitarsis',
   'peach borer',
   'english grain aphid',
   'green bug',
   'bird cherry-oataphid',
   'wheat blossom midge',
   'penthaleus major',
   'longlegged spider mite',
   'wheat phloeothrips',
   'wheat sawfly',
   'cerodonta denticornis',
   'beet fly',
   'flea beetle',
   'cabbage army worm',
   'beet army worm',
   'Beet spot flies',
   'meadow moth',
   'beet weevil',
   'sericaorient alismots chulsky',
   'alfalfa weevil',
   'flax budworm',
   'alfalfa plant bug',
   'tarnished plant bug',
   'Locustoidea',
   'lytta polita',
   'legume blister beetle',
   'blister beetle',
   'therioaphis maculata Buckton',
   'odontothrips loti',
   'Thrips',
   'alfalfa seed chalcid',
   'Pieris canidia',
   'Apolygus lucorum',
   'Limacodidae',
   'Viteus vitifoliae',
   'Colomerus vitis',
   'Brevipoalpus lewisi McGregor',
   'oides decempunctata',
   'Polyphagotars onemus latus',
   'Pseudococcus comstocki Kuwana',
   'parathrene regalis',
   'Ampelophaga',
   'Lycorma delicatula',
   'Xylotrechus',
   'Cicadella viridis',
   'Miridae',
   'Trialeurodes vaporariorum',
   'Erythroneura apicalis',
   'Papilio xuthus',
   'Panonchus citri McGregor',
   'Phyllocoptes oleiverus ashmead',
   'Icerya purchasi Maskell',
   'Unaspis yanonensis',
   'Ceroplastes rubens',
   'Chrysomphalus aonidum',
   'Parlatoria zizyphus Lucus',
   'Nipaecoccus vastalor',
   'Aleurocanthus spiniferus',
   'Tetradacus c Bactrocera minax',
   'Dacus dorsalis(Hendel)',
   'Bactrocera tsuneonis',
   'Prodenia litura',
   'Adristyrannus',
   'Phyllocnistis citrella Stainton',
   'Toxoptera citricidus',
   'Toxoptera aurantii',
   'Aphis citricola Vander Goot',
   'Scirtothrips dorsalis Hood',
   'Dasineura sp',
   'Lawana imitata Melichar',
   'Salurnis marginella Guerr',
   'Deporaus marginatus Pascoe',
   'Chlumetia transversa',
   'Mango flat beak leafhopper',
   'Rhytidodera bowrinii white',
   'Sternochetus frigidus',
   'Cicadellidae'],
  'scores': [],
  'bounding_boxes': [],
  'save_path': 's3://ndvi-images-bucket/outputs/yolo_2025-04-19T18:37:44.241214_20180711_seq_50m_NC (1).tif'},
 'weather_data': {'location': 'Lat: 37.7749, Lon: -122.4194',
  'past_7_days': ['2025-04-20: Temp 9.5-18.3°C, Humidity 83%, Rain 0.0mm, Wind Max 19.0 km/h, ETo 2.98mm',
   '2025-04-21: Temp 7.9-21.6°C, Humidity 78%, Rain 0.0mm, Wind Max 24.1 km/h, ETo 4.11mm',
   '2025-04-22: Temp 9.2-17.6°C, Humidity 83%, Rain 0.0mm, Wind Max 25.3 km/h, ETo 3.07mm',
   '2025-04-23: Temp 9.5-14.5°C, Humidity 81%, Rain 0.0mm, Wind Max 20.5 km/h, ETo 2.04mm',
   '2025-04-24: Temp 9.7-13.5°C, Humidity 79%, Rain 0.0mm, Wind Max 19.6 km/h, ETo 1.51mm',
   '2025-04-25: Temp 9.2-14.8°C, Humidity 74%, Rain 0.0mm, Wind Max 15.6 km/h, ETo 2.22mm',
   '2025-04-26: Temp 7.0-14.8°C, Humidity 81%, Rain 0.0mm, Wind Max 19.5 km/h, ETo 2.45mm'],
  'current': {'temp': '10.7°C', 'wind': '3.7 km/h'},
  'forecast_next_days': ['2025-04-27: Temp 9.7-14.2°C, Humidity 83%, Rain 0.0mm, Wind Max 23.8 km/h, ETo 2.61mm',
   '2025-04-28: Temp 7.7-18.1°C, Humidity 77%, Rain 0.0mm, Wind Max 22.5 km/h, ETo 4.02mm',
   '2025-04-29: Temp 10.3-21.8°C, Humidity 71%, Rain 0.0mm, Wind Max 16.7 km/h, ETo 4.52mm']},
 'recommendation': {'risk_level': 'Moderate',
  'advice': 'Monitor your crops closely for signs of pest infestations. Consider applying an organic insecticide like neem oil or a targeted chemical pesticide to control any detected pests. Maintain consistent irrigation and monitor weather conditions to ensure optimal crop health.',
  'data_summary': {'summary': 'While no NDVI data is available, the weather conditions over the past week indicate moderate risk. Pests may be present, so vigilant monitoring and appropriate pest control measures are recommended.'}},
 'image_key': 'ndvi_results/2025-04-19T18:37:44.241214_20180711_seq_50m_NC (1).tif',
 'coordinates': {'latitude': 37.7749, 'longitude': -122.4194}}

```


## 🪪 License
Licensed under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0).



## 🤝 Contributing
Pull requests and issue reports are welcome. If you're working on precision agriculture, satellite imagery, or rural tech—let's collaborate.