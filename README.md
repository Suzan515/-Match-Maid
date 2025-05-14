import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:cloud_firestore/cloud_firestore.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MatchMaidApp());
}

class MatchMaidApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Match-Maid',
      theme: ThemeData(
        primarySwatch: Colors.purple,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      debugShowCheckedModeBanner: false,
      home: WelcomeScreen(),
    );
  }
}

// Welcome Screen
class WelcomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(24.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text(
                'Match-Maid',
                style: TextStyle(fontSize: 32, fontWeight: FontWeight.bold),
              ),
              SizedBox(height: 20),
              ElevatedButton(
                onPressed: () {
                  Navigator.push(
                    context,
                    MaterialPageRoute(builder: (context) => LoginScreen()),
                  );
                },
                child: Text('Get Started'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// Login Screen
class LoginScreen extends StatelessWidget {
  final TextEditingController phoneController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Login')),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(
              controller: phoneController,
              keyboardType: TextInputType.phone,
              decoration: InputDecoration(
                labelText: 'Phone Number',
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) => RoleSelectionScreen()),
                );
              },
              child: Text('Continue'),
            ),
          ],
        ),
      ),
    );
  }
}

// Role Selection Screen
class RoleSelectionScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Select Role')),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) => MaidProfileForm()),
                );
              },
              child: Text("I'm a Maid"),
              style: ElevatedButton.styleFrom(minimumSize: Size(double.infinity, 50)),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) => EmployerProfileForm()),
                );
              },
              child: Text("I'm a Maid Founder"),
              style: ElevatedButton.styleFrom(minimumSize: Size(double.infinity, 50)),
            ),
          ],
        ),
      ),
    );
  }
}

// Maid Profile Form
class MaidProfileForm extends StatelessWidget {
  final _formKey = GlobalKey<FormState>();
  final nameController = TextEditingController();
  final ageController = TextEditingController();
  final locationController = TextEditingController();
  final experienceController = TextEditingController();
  final servicesController = TextEditingController();

  void saveMaidProfile() async {
    await FirebaseFirestore.instance.collection('maids').add({
      'name': nameController.text,
      'age': ageController.text,
      'location': locationController.text,
      'experience': experienceController.text,
      'services': servicesController.text,
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Maid Profile')),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Form(
          key: _formKey,
          child: ListView(
            children: [
              TextFormField(
                controller: nameController,
                decoration: InputDecoration(labelText: 'Name'),
              ),
              TextFormField(
                controller: ageController,
                decoration: InputDecoration(labelText: 'Age'),
              ),
              TextFormField(
                controller: locationController,
                decoration: InputDecoration(labelText: 'Location'),
              ),
              TextFormField(
                controller: experienceController,
                decoration: InputDecoration(labelText: 'Experience'),
              ),
              TextFormField(
                controller: servicesController,
                decoration: InputDecoration(labelText: 'Services Offered'),
              ),
              SizedBox(height: 20),
              ElevatedButton(
                onPressed: () {
                  if (_formKey.currentState!.validate()) {
                    saveMaidProfile();
                  }
                },
                child: Text('Submit'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// Employer Profile Form
class EmployerProfileForm extends StatelessWidget {
  final _formKey = GlobalKey<FormState>();
  final nameController = TextEditingController();
  final locationController = TextEditingController();
  final requirementsController = TextEditingController();

  void saveEmployerProfile() async {
    await FirebaseFirestore.instance.collection('employers').add({
      'name': nameController.text,
      'location': locationController.text,
      'requirements': requirementsController.text,
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Employer Profile')),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Form(
          key: _formKey,
          child: ListView(
            children: [
              TextFormField(
                controller: nameController,
                decoration: InputDecoration(labelText: 'Name'),
              ),
              TextFormField(
                controller: locationController,
                decoration: InputDecoration(labelText: 'Location'),
              ),
              TextFormField(
                controller: requirementsController,
                decoration: InputDecoration(labelText: 'Job Requirements'),
              ),
              SizedBox(height: 20),
              ElevatedButton(
                onPressed: () {
                  if (_formKey.currentState!.validate()) {
                    saveEmployerProfile();
                  }
                },
                child: Text('Submit'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

