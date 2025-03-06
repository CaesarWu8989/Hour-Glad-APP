import SwiftUI

struct SandEntry: Identifiable, Equatable {
    let id = UUID()
    let color: Color
    let hours: Int
}



struct MainAppView: View {
    @State private var sandLevels: [String: [SandEntry]] = [:]
    @State private var categories: [String] = ["Family"]
    @State private var categoryColors: [String: Color] = ["Family": .red]
    @State private var selectedCategoryIndex = 0
    @State private var isAnimating = false
    @State private var newCategoryName: String = ""
    @State private var selectedColor: Color = .red
    @State private var showingCategoryInput = false
    @State private var showingColorPicker = false
    @State private var isFlipped = false
    @State private var sandFlowing = false
    @State private var totalHours: Int = 0
    
    var body: some View {
        ZStack {
            Color.black.edgesIgnoringSafeArea(.all)
            VStack {
                Text(categories[selectedCategoryIndex])
                    .font(.title)
                    .bold()
                    .foregroundColor(.white)
                    .padding()
                
                Text("Total Hours: \(totalHours)")
                    .font(.headline)
                    .foregroundColor(.white)
                
                HStack {
                    Button(action: {
                        if selectedCategoryIndex > 0 {
                            selectedCategoryIndex -= 1
                        }
                    }) {
                        Image(systemName: "arrow.left")
                            .font(.title)
                            .foregroundColor(.white)
                            .padding()
                            .background(Color.black)
                            .clipShape(Circle())
                            .shadow(color: .gray, radius: 5, x: 0, y: 5)
                    }
                    
                    Button(action: {
                        showingCategoryInput = true
                    }) {
                        Image(systemName: "arrow.right")
                            .font(.title)
                            .foregroundColor(.white)
                            .padding()
                            .background(Color.black)
                            .clipShape(Circle())
                            .shadow(color: .gray, radius: 5, x: 0, y: 5)
                    }
                }
                
                ZStack {
                    SandStackView(sandLevels: $sandLevels, category: categories[selectedCategoryIndex], isFlipped: $isFlipped)
                        .animation(.easeIn(duration: 2), value: sandLevels)
                    
                    Image("Hourglad transparent.001")
                        .resizable()
                        .scaledToFit()
                        .frame(width: 300, height: 600)
                        .opacity(1.0)
                }
                .rotationEffect(isFlipped ? .degrees(180) : .degrees(0))
                
                Button("Input Sand") {
                    let category = categories[selectedCategoryIndex]
                    let color = categoryColors[category] ?? .gray
                    sandLevels[category, default: []].append(SandEntry(color: color, hours: 1))
                    totalHours += 1
                }
                .padding()
                .background(Color.black)
                .cornerRadius(15)
                .foregroundColor(.white)
                .shadow(color: .gray, radius: 5, x: 0, y: 5)
                
                Button("Start") {
                    isFlipped.toggle()
                    sandFlowing.toggle()
                }
                .padding()
                .background(Color.black)
                .cornerRadius(15)
                .foregroundColor(.white)
                .shadow(color: .gray, radius: 5, x: 0, y: 5)
            }
        }
        .sheet(isPresented: $showingCategoryInput) {
            VStack {
                TextField("Enter Name", text: $newCategoryName)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                    .padding()
                Button("Select Color") {
                    showingColorPicker = true
                }
                .padding()
                .background(Color.white)
                .cornerRadius(10)
                .foregroundColor(.black)
                .sheet(isPresented: $showingColorPicker) {
                    ColorPicker("Pick a Color", selection: $selectedColor)
                        .padding()
                }
                Button("Confirm") {
                    categories.append(newCategoryName)
                    categoryColors[newCategoryName] = selectedColor
                    sandLevels[newCategoryName] = []
                    selectedCategoryIndex = categories.count - 1
                    showingCategoryInput = false
                }
                .padding()
                .background(Color.white)
                .cornerRadius(10)
                .foregroundColor(.black)
            }
            .padding()
        }
    }
}

struct SandStackView: View {
    @Binding var sandLevels: [String: [SandEntry]]
    var category: String
    @Binding var isFlipped: Bool
    
    var body: some View {
        VStack(spacing: 0) {
            ForEach(sandLevels[category, default: []], id: \ .id) { entry in
                WaveShape()
                    .fill(LinearGradient(gradient: Gradient(colors: [entry.color, entry.color.opacity(0.5)]), startPoint: .top, endPoint: .bottom))
                    .frame(height: CGFloat(entry.hours) * 40)
                    .transition(.opacity)
            }
        }
        .frame(width: 230, height: 400, alignment: isFlipped ? .top : .bottom)
        .clipShape(Rectangle())
    }
}

struct WaveShape: Shape {
    func path(in rect: CGRect) -> Path {
        var path = Path()
                let waveHeight: CGFloat = 20 // 更細的波浪
                path.move(to: CGPoint(x: 0, y: rect.maxY))
                for x in stride(from: 0, to: rect.width, by: 5) {
                    let y = sin((x / rect.width) * .pi * 2) * waveHeight + rect.maxY - waveHeight
                    path.addLine(to: CGPoint(x: x, y: y))
                }
                path.addLine(to: CGPoint(x: rect.width, y: rect.maxY))
                path.addLine(to: CGPoint(x: 0, y: rect.maxY))
                path.closeSubpath()
                return path
    }
}
