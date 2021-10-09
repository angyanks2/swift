# swift
Study Pal Chat App Edit Files
//
//  ContentView.swift
//  STUDYPALCHATAPP
//
//  Created by Anthony Guerrera on 1/28/21.
//

//
//  ContentView.swift
//  StudyPal-ChatApp
//
//  Created by Anthony Guerrera on 8/16/20.
//  Copyright © 2020 Fupa Tech. All rights reserved.
//

import SwiftUI
import Firebase

@available(iOS 14.0, *)
struct ContentView: View {
    @AppStorage("log_Status") var status = false
    @StateObject var model = ModelData()
    var body: some View {
      
        ZStack{
                      
                      if status{
                          
                          VStack(spacing: 25){
                              
                              Text("Logged In As \(Auth.auth().currentUser?.email ?? "")")
                              
                              Button(action: model.logOut, label: {
                                  Text("LogOut")
                                      .fontWeight(.bold)
                              })
                          }
                      }
                      else{
                          
                          LoginView(model: model)
                      }
                  }
              }
      }

struct DeBugView: View {
    var body: some View {
        VStack {
        
        Image("daddyclogo")
            .resizable()
            .frame(width: 200, height: 200)
        .cornerRadius(30)
        .padding(.horizontal)
        .padding(.vertical,20)
        
        .shadow(color: Color.black, radius: 20)
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    
    static var previews: some View {
        if #available(iOS 14.0, *) {
            ContentView()
        } else {
            // Fallback on earlier versions
        }
    }
}

struct LoginView : View {
    @available(iOS 14.0, *)
    @ObservedObject var model : ModelData
    
    var body: some View {
        ZStack{
                VStack {
                    
            
                    Spacer(minLength: 0)
                    
                    
                        
                    
                    Image("daddyclogo")
                    .resizable()
                    .frame(width: 200, height: 200)
                    .cornerRadius(30)
                    .padding(.horizontal)
                    .padding(.vertical,20)
                    
                    .shadow(color: Color.black, radius: 20)
                    
                    
                    
                    
                    VStack(spacing: 4) {
                        HStack(spacing: 0 ) {
                            Text("Study").font(.system(size: 35, weight: .heavy))
                                .foregroundColor(.black)
                            Text("Pal").font(.system(size: 35, weight: .heavy))
                                .foregroundColor(Color("peach").opacity(1.0))
                            
                        }
                        
                        Text("Study Chat App")
                            .foregroundColor(Color.black)
                            .fontWeight(.heavy)
                    }.padding(.top)
                        
                    
                    VStack(spacing: 20) {
                        CustomTextField(placeholder: "Email",  txt: $model.email)
                        
                        CustomTextField(placeholder: "Password", txt: $model.password)
                    }.padding(.top)
                    
                    Button(action: {
                        // to do...
                        // connect to firebase database...
                        if #available(iOS 14.0, *) {
                            self.model.login()
                        } else {
                            // Fallback on earlier versions
                        }
                    }) {
                        Text("LOGIN")
                            .fontWeight(.bold)
                            .foregroundColor(Color("hotpink"))
                            .padding(.vertical)
                            .frame(width: UIScreen.main.bounds.width - 30)
                            .background(Color.white)
                            .clipShape(Capsule())
                        
                    }.padding(.top, 22)
                    
                    HStack(spacing: 12) {
                        Text("Don't have an account?")
                            
                            .foregroundColor(Color.white.opacity(0.7))
                            
                        Button(action: {
                            if #available(iOS 14.0, *) {
                                self.model.isSignUp.toggle()
                            } else {
                                // Fallback on earlier versions
                            }
                        }) {
                            Text("Sign Up Now")
                                .fontWeight(.bold)
                                .foregroundColor(.white)
                        
                        }
                    }
                    .padding(.top,25)
                
                    Spacer(minLength: 0)
                    
                    Button(action:{if #available(iOS 14.0, *) {
                        self.model.resetPassword()
                    } else {
                        // Fallback on earlier versions
                    }}) {
                                        
                        Text("Forget Password?")
                            .fontWeight(.bold)
                            .foregroundColor(.white)
                    }
                    .padding(.vertical,22)
                    
                    
                
                    
                    
                }
            if #available(iOS 14.0, *) {
                if model.isLoading {
                    LoadingView()
                }
            } else {
                // Fallback on earlier versions
            }
        }
        .background(LinearGradient(gradient: .init(colors: [Color("hotpink"), Color.black]), startPoint: .top, endPoint: .bottom).edgesIgnoringSafeArea(.all))
        .sheet(isPresented: $model.isSignUp) {
            
            if #available(iOS 14.0, *) {
                SignUpView(model: self.model)
            } else {
                // Fallback on earlier versions
            }
        }
        // Alerts...
        .alert(isPresented: $model.alert, content: {
            
            Alert(title: Text("Message"), message: Text(model.alertMsg), dismissButton: .destructive(Text("Ok")))
        })
        
        
        
        
        
        
    }
        
        
        
        
}

struct CustomTextField : View {
    
    
    var placeholder: String
    @State var passIsVisible : Bool = false
    @Binding var txt : String
    
    
    var body: some View {
        
        ZStack(alignment: Alignment(horizontal: .leading, vertical: .center)) {
            ZStack{
                        
                        if placeholder == "Email" || placeholder == "Re-Enter" {
                            TextField(placeholder, text: $txt)
                        }
                        else{
                            if !self.passIsVisible {
                                SecureField(placeholder, text: $txt)
                            } else {
                                TextField(placeholder, text: $txt)
                            }
                        }
                    }
            
                        .padding(.horizontal)
                        .padding(.leading, 65)
                        .frame(height: 60)
                        .background(Color.white.opacity(0.3))
                        .clipShape(Capsule())
                
            
            
            
            
            if placeholder == "Email" {
                Image(systemName: "person")
                .font(.system(size: 24))
                .foregroundColor(.black)
                .frame(width: 60, height: 60)
                .background(Color.white)
                .clipShape(Circle())
            } else {
                
            
                Button(action: {
                    self.passIsVisible.toggle()
                    print("Hii mowgli")
                }) {
                    Image(systemName: self.passIsVisible ? "eye.fill" : "eye.slash.fill")
                    .font(.system(size: 24))
                    .foregroundColor(.black)
                    .frame(width: 60, height: 60)
                    .background(Color.white)
                    .clipShape(Circle())
                }
            }
            
    
        
        }.padding(.horizontal)
    }
}

@available(iOS 14.0, *)
class ModelData: ObservableObject {
    @Published var email = ""
    @Published var password = ""
    @Published var isSignUp = false
    @Published var email_SignUp = ""
    @Published var password_SignUp = ""
    @Published var reEnterPassword = ""
    @Published var resetEmail = ""
    @Published var alert = false
    @Published var alertMsg = ""
    @Published var isLoading = false
    @AppStorage("log_Status") var status = false
  
   
    
    
    // Reset Password Functionality
    func resetPassword(){
             
             let alert = UIAlertController(title: "Reset Password", message: "Enter Your E-Mail ID To Reset Your Password", preferredStyle: .alert)
             
             alert.addTextField { (password) in
                 password.placeholder = "Email"
             }
            
            let proceed = UIAlertAction(title: "Reset", style: .default) { (_) in
                // Sending Password Link...
                              
                              if alert.textFields![0].text! != ""{
                                  
                                  withAnimation{
                                      
                                      self.isLoading.toggle()
                                  }
                                  
                                  Auth.auth().sendPasswordReset(withEmail: alert.textFields![0].text!) { (err) in
                                      
                                      withAnimation{
                                          
                                          self.isLoading.toggle()
                                      }
                                      
                                      if err != nil{
                                          self.alertMsg = err!.localizedDescription
                                          self.alert.toggle()
                                          return
                                      }
                                      
                                      // ALerting User...
                                      self.alertMsg = "Password Reset Link Has Been Sent !!!"
                                      self.alert.toggle()
                                  }
                              }
                          }
            let cancel = UIAlertAction(title: "Cancel", style: .destructive, handler: nil)
                
                alert.addAction(cancel)
                alert.addAction(proceed)
                
                
                // Presenting...
                
                UIApplication.shared.windows.first?.rootViewController?.present(alert, animated: true)
        }
    func login() {
        // checking all fields are inputted correctly...
        
        if self.email == "" || self.password == ""{
            
            self.alertMsg = "Please fill contents properly"
            self.alert.toggle()
            return
        }
        
        withAnimation{
            
            self.isLoading.toggle()
        }
        
        Auth.auth().signIn(withEmail: email, password: password) { (result, err) in
            
            withAnimation{
                
                self.isLoading.toggle()
            }
            
            if err != nil{
                
                self.alertMsg = "There is no current user under this account. Please format correctly or sign up."
                self.alert.toggle()
                return
            }
            
            // checking if user is verifed or not...
            // if not verified means logging out...
            
            let user = Auth.auth().currentUser
            
            if !user!.isEmailVerified{
                
                self.alertMsg = "Please Verify Email Address"
                self.alert.toggle()
                // logging out...
                try! Auth.auth().signOut()
                
                return
            }
            
            // setting user status as true....
            
            withAnimation{
                
                self.status = true
            }
        }
        
        
        
    }
    
    func signUp() {
        
        // checking....
                  
                  if email_SignUp == "" || password_SignUp == "" || reEnterPassword == ""{
                      
                      self.alertMsg = "Fill contents proprely!!!"
                      self.alert.toggle()
                      return
                  }
                  
                  if password_SignUp != reEnterPassword{
                      
                      self.alertMsg = "Password Mismatch !!!"
                      self.alert.toggle()
                      return
                  }
                  
                  withAnimation{
                      
                      self.isLoading.toggle()
                  }
                  
                  Auth.auth().createUser(withEmail: email_SignUp, password: password_SignUp) { (result, err) in
                      
                      withAnimation{
                          
                          self.isLoading.toggle()
                      }
                      
                      if err != nil{
                          self.alertMsg = err!.localizedDescription
                          self.alert.toggle()
                          return
                      }
                      
                      // sending Verifcation Link....
                      
                      result?.user.sendEmailVerification(completion: { (err) in
                          
                          if err != nil{
                              self.alertMsg = err!.localizedDescription
                              self.alert.toggle()
                              return
                          }
                          
                          // Alerting User To Verify Email...
                          
                          self.alertMsg = "Email Verification Has Been Sent !!! Verify Your Email ID !!!"
                          self.alert.toggle()
                      })
                  }
        
        
        
    }
    
func logOut(){
    try! Auth.auth().signOut()
              
              withAnimation{
                  
                  self.status = false
              }
              
              // clearing all data...
              
              email = ""
              password = ""
              email_SignUp = ""
              password_SignUp = ""
              reEnterPassword = ""
          }
}
struct SignUpView : View {
    
    @available(iOS 14.0, *)
    @ObservedObject var model : ModelData
    var body: some View {
        ZStack(alignment: Alignment(horizontal: .trailing, vertical: .top), content: {
        VStack {
            
            Spacer(minLength: 0)
            
            Image("daddyclogo")
            .resizable()
                .frame(width: 200, height: 200)
            .cornerRadius(30)
            .padding(.horizontal)
            .padding(.vertical,20)
            
            .shadow(color: Color.black, radius: 20)
                
            VStack(spacing: 4) {
                HStack(spacing: 0 ) {
                    Text("New").font(.system(size: 35, weight: .heavy))
                        .foregroundColor(.black)
                    Text("Profile").font(.system(size: 35, weight: .heavy))
                        .foregroundColor(Color("peach").opacity(1.0))
                    
                }
                
                Text("All your courses in one place")
                    .foregroundColor(Color.black)
                    .fontWeight(.heavy)
            }.padding(.top)
                
            
            VStack(spacing: 20) {
                CustomTextField(placeholder: "Email", txt: $model.email_SignUp)
                
                CustomTextField(placeholder: "Password", txt: $model.password_SignUp)
                
                CustomTextField(placeholder: "Re-Enter", txt: $model.reEnterPassword)
            }.padding(.top)
            
            Button(action: {if #available(iOS 14.0, *) {
                self.model.signUp()
            } else {
                // Fallback on earlier versions
            }}) {
                
                Text("SIGNUP")
                    .fontWeight(.bold)
                    .foregroundColor(Color("hotpink"))
                    .padding(.vertical)
                    .frame(width: UIScreen.main.bounds.width - 30)
                    .background(Color.white)
                    .clipShape(Capsule())
            }
            .padding(.vertical,22)
            
            Spacer(minLength: 0)
            
        }
            Button(action: {if #available(iOS 14.0, *) {
                self.model.isSignUp.toggle()
            } else {
                // Fallback on earlier versions
            }}) {
            
            Image(systemName: "xmark")
                .foregroundColor(.white)
                .padding()
                .background(Color.black.opacity(0.6))
                .clipShape(Circle())
        }
        .padding(.trailing)
        .padding(.top,10)
        
            if model.isLoading {
                LoadingView()
            }
        
            }).background(LinearGradient(gradient: .init(colors: [Color("hotpink"),Color.black]), startPoint: .top, endPoint: .bottom).edgesIgnoringSafeArea(.all))
        .alert(isPresented: $model.alert, content: {
                      
                      Alert(title: Text("Message"), message: Text(model.alertMsg), dismissButton: .destructive(Text("Ok"), action: {
                           
                          // if email link sent means closing the signupView....
                          
                          if model.alertMsg == "Email Verification Has Been Sent !!! Verify Your Email ID !!!"{
                              
                              model.isSignUp.toggle()
                              model.email_SignUp = ""
                              model.password_SignUp = ""
                              model.reEnterPassword = ""
                          }
                          
                      }))
                  })
        
    }
}
// Loading animation view
struct LoadingView : View {
      
      @State var animation = false
      
      var body: some View{
          
          VStack{
              
              Circle()
                  .trim(from: 0, to: 0.7)
                  .stroke(Color("hotpink"),lineWidth: 8)
                  .frame(width: 75, height: 75)
                  .rotationEffect(.init(degrees: animation ? 360 : 0))
                  .padding(50)
          }
          .background(Color.white)
          .cornerRadius(20)
          .frame(maxWidth: .infinity, maxHeight: .infinity)
          .background(Color.black.opacity(0.4).edgesIgnoringSafeArea(.all))
          .onAppear(perform: {
              
              withAnimation(Animation.linear(duration: 5)){
                  
                  animation.toggle()
              }
          })
        }
  
}
   
