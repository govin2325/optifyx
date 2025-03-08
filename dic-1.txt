import React, { useState } from 'react';
import { Card, CardHeader, CardTitle, CardContent } from '@/components/ui/card';
import { Upload } from 'lucide-react';

const SkinToneAnalyzer = () => {
  const [selectedFile, setSelectedFile] = useState(null);
  const [imagePreview, setImagePreview] = useState(null);
  const [analysis, setAnalysis] = useState(null);

  const skinTones = {
    'Fair': { hex: '#FBEEE3', number: 1.5 },
    'Porcelain': { hex: '#F6E2D3', number: 2.5 },
    'Ivory': { hex: '#F2D5C4', number: 3.5 },
    'Sand': { hex: '#E8C5B3', number: 4.5 },
    'Beige': { hex: '#E4B89F', number: 5.5 },
    'Golden': { hex: '#DEBA9F', number: 6.5 },
    'Honey': { hex: '#D39B7A', number: 7.5 },
    'Bronze': { hex: '#C69076', number: 8.5 },
    'Almond': { hex: '#B67B5E', number: 9.5 },
    'Chestnut': { hex: '#9F6548', number: 10.5 },
    'Deep': { hex: '#8B4E33', number: 11.5 },
    'Espresso': { hex: '#613025', number: 12.5 }
  };

  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      setSelectedFile(file);
      const reader = new FileReader();
      reader.onloadend = () => {
        setImagePreview(reader.result);
        // Simulate analysis (in real implementation, this would call the Python backend)
        analyzeSkinTone(reader.result);
      };
      reader.readAsDataURL(file);
    }
  };

  const analyzeSkinTone = (imageData) => {
    // Simulated analysis result
    // In a real implementation, this would call your Python backend
    setTimeout(() => {
      setAnalysis({
        detected_color: '#8B4E33',
        closest_match: {
          name: 'Deep',
          number: 11.5,
          reference_hex: '#8B4E33'
        }
      });
    }, 1000);
  };

  return (
    <Card className="w-full max-w-2xl">
      <CardHeader>
        <CardTitle>Skin Tone Analyzer</CardTitle>
      </CardHeader>
      <CardContent>
        <div className="space-y-6">
          {/* Upload Section */}
          <div className="border-2 border-dashed border-gray-300 rounded-lg p-6 text-center">
            <input
              type="file"
              accept="image/*"
              onChange={handleFileChange}
              className="hidden"
              id="image-upload"
            />
            <label 
              htmlFor="image-upload" 
              className="cursor-pointer flex flex-col items-center space-y-2"
            >
              <Upload className="w-12 h-12 text-gray-400" />
              <span className="text-sm text-gray-600">
                Click to upload or drag and drop
              </span>
              <span className="text-xs text-gray-500">
                PNG, JPG up to 10MB
              </span>
            </label>
          </div>

          {/* Preview Section */}
          {imagePreview && (
            <div className="space-y-4">
              <h3 className="font-medium">Image Preview</h3>
              <div className="flex justify-center">
                <img 
                  src={imagePreview} 
                  alt="Preview" 
                  className="max-h-48 rounded-lg"
                />
              </div>
            </div>
          )}

          {/* Results Section */}
          {analysis && (
            <div className="space-y-4 border-t pt-4">
              <h3 className="font-medium">Analysis Results</h3>
              <div className="grid grid-cols-2 gap-4">
                <div className="space-y-2">
                  <p className="text-sm font-medium">Detected Color</p>
                  <div 
                    className="w-16 h-16 rounded-lg border"
                    style={{ backgroundColor: analysis.detected_color }}
                  />
                  <p className="text-sm text-gray-600">
                    {analysis.detected_color}
                  </p>
                </div>
                <div className="space-y-2">
                  <p className="text-sm font-medium">Closest Match</p>
                  <div 
                    className="w-16 h-16 rounded-lg border"
                    style={{ backgroundColor: analysis.closest_match.reference_hex }}
                  />
                  <p className="text-sm text-gray-600">
                    {analysis.closest_match.name}
                  </p>
                  <p className="text-sm text-gray-600">
                    Tone Number: {analysis.closest_match.number}
                  </p>
                </div>
              </div>
            </div>
          )}

          {/* Reference Colors */}
          <div className="border-t pt-4">
            <h3 className="font-medium mb-4">Reference Skin Tones</h3>
            <div className="grid grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-4">
              {Object.entries(skinTones).map(([name, data]) => (
                <div key={name} className="text-center space-y-2">
                  <div 
                    className="w-12 h-12 rounded-lg border mx-auto"
                    style={{ backgroundColor: data.hex }}
                  />
                  <p className="text-xs font-medium">{name}</p>
                  <p className="text-xs text-gray-600">{data.number}</p>
                </div>
              ))}
            </div>
          </div>
        </div>
      </CardContent>
    </Card>
  );
};

export default SkinToneAnalyzer;