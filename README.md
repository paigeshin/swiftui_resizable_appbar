# swiftui_resizable_appbar

```swift
//
//  ContentView.swift
//  ResizableAppBar
//
//  Created by paige shin on 2023/06/11.
//

import SwiftUI

struct ContentView: View {
    var body: some View {
        Home()
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

struct Home: View {
    
    @State var top = UIApplication.shared.windows.first?.safeAreaInsets.top
    @State var current = "house.fill"
    @Namespace var animation
    @State var isHide = false
    
    var body: some View {
        VStack(spacing: 0) {
            
            // App Bar
            VStack(spacing: 22) {
                if !self.isHide {
                    HStack(spacing: 12) {
                        Text("faceebook")
                            .font(.largeTitle)
                            .fontWeight(.heavy)
                            .foregroundColor(.blue)
                            
                        
                        Spacer()
                        
                        Button {
                            
                        } label: {
                            Image(systemName: "magnifyingglass")
                                .foregroundColor(.black)
                                .padding(10)
                                .background(Color.black.opacity(0.1))
                                .clipShape(Circle())
                        }
                        
                        Button {
                            
                        } label: {
                            Image(systemName: "message.fill")
                                .foregroundColor(.black)
                                .padding(10)
                                .background(Color.black.opacity(0.1))
                                .clipShape(Circle())
                        }

                    } //: HSTACK
                    .padding(.horizontal)
                }
        
                
                // Custom Tab Bar...
                HStack(spacing: 0) {
                    TabBarButton(current: self.$current,
                                 image: "house.fill",
                                 animation: self.animation)
                    TabBarButton(current: self.$current,
                                 image: "magnifyingglass",
                                 animation: self.animation)
                    TabBarButton(current: self.$current,
                                 image: "person.2.fill",
                                 animation: self.animation)
                    TabBarButton(current: self.$current,
                                 image: "tv.fill",
                                 animation: self.animation)
                } //: HSTACK
            } //: VSTACK
            .padding(.top, self.top)
            .background(Color.white)
            
            ScrollView(.vertical, showsIndicators: false) {
                VStack(spacing: 0) {
              
                    // Geometry Reader for getting location values...
                    GeometryReader { reader -> AnyView in
                        
                        let yAxis = reader.frame(in: .global).minY
                        
                        // logic is simple if it goes below zero hide nav bar.
                        // above zero show navbar..
                        
                        if yAxis < 0 && !self.isHide {
                            DispatchQueue.main.async {
                                withAnimation {
                                    self.isHide = true
                                }
                            }
                        }
                        
                        if yAxis > 0 && self.isHide {
                            DispatchQueue.main.async {
                                withAnimation {
                                    self.isHide = false
                                }
                            }
                        }
                        
                        return AnyView(
                            Text("")
                                .frame(width: 0, height: 0)
                        )
                    } //: GEO
                    
                    VStack(spacing: 15) {
                        ForEach(1...20, id: \.self) { i in
                            VStack(spacing: 10) {
                                HStack(spacing: 10) {
                                    Image(systemName: "heart.fill")
                                        .resizable()
                                        .frame(width: 35, height: 35)
                                    
                                    VStack(alignment: .leading, spacing: 10) {
                                        Text("Paige Software")
                                            .font(.title2)
                                            .fontWeight(.semibold)
                                            .foregroundColor(.black)
                                        
                                        Text("\(45 - i) Min")
                                    } //: VSTACK
                                    Spacer(minLength: 0)
                                } //: HSTACK
                                
                                Text("Lorem Ipsum is simply dummy text of the printing and typesetting industry.")
                                    .frame(maxWidth: .infinity, alignment: .leading)
                                
                            } //: VSTACK
                            .padding()
                            .background(Color.white)
                        }
                    }
                    
                } //: VSTACK
                .padding(.top)
            } //: ScrollView
            
        } //: VSTACK
        .background(Color.black.opacity(0.07))
        .ignoresSafeArea()
    }
    
}

struct TabBarButton: View {
    
    @Binding var current: String
    var image: String
    var animation: Namespace.ID
    
    var body: some View {
        Button(action: {
            withAnimation {
                self.current = self.image
            }
        }) {
            VStack(spacing: 5) {
                Image(systemName: self.image)
                    .font(.title2)
                    .foregroundColor(self.current == image ? Color.blue : Color.black.opacity(0.3))
                // Default frame to avoid resizing...
                    .frame(height: 35)
                
                ZStack {
                    Rectangle()
                        .fill(Color.clear)
                        .frame(height: 4)
                    
                    // Matched Geometry Effect Slide Animation...
                    
                    if self.current == self.image {
                        Rectangle()
                            .fill(Color.blue)
                            .frame(height: 4)
                            .matchedGeometryEffect(id: "Tab", in: self.animation)
                    }
                    
                } //: ZSTACK
            } //: VSTACK
        }
    }
    
}

```
