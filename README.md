# Airfreight Logistics Manager

###### Manage and record logistics data of Containers and Pallets ready to be loaded into a plane’s haul with  weight compliance monitoring

![Software Demo](https://github.com/jaaaron/Airfreight-Logistics-Manager/blob/main/airExample.gif)

```
#include <iostream>
#include <string>
#include <iomanip>
#include <fstream>
#include <vector>
using namespace std;
const int maxload737 = 46000;
const int maxload767 = 116000;
string plane767[]={"AKE","APE","AKC","AQP","AQF""P1P","AAP","P6P"};

class Cargo
{
protected:
  string uldtype;     // unit load type, container or pallet
  string abbrev;      // three letters AYF, PAG etc.
  string uldid;       // unit id
  int aircraft;       // aircraft model number
  double weight;      // weight in pounds
  string destination; // airport
public:
  Cargo();  // default constructor prototype
  Cargo(const string &uldtype,
        const string &abbrev,
        const string &uldid,
        const int &aircraft,
        const double &weight,
        const string &destination); //full constructor prototype

  Cargo(const Cargo &unit1); // Copy Constructor prototype
    
  // Destructor prototype
  ~Cargo ();
  //Mutator
  void setuldtype(string);
  void setabbrev(string);
  void setuldid(string);
  void setaircraft(int);
  void setweight(double);
  void setdestination(string);
  //Accessor
  string getuldtype() const;
  string getabbrev() const;
  string getuldid() const;
  int getaircraft() const;
  double getweight() const;
  string getdestination() const;
  virtual bool maxweight(int)
  {
  return 0;
  }

  virtual void output();
};

class Boeing737: public Cargo
{
private:
public:
  //derived fault constructor
  Boeing737()
  {
    Cargo();
  }
  Boeing737(const Cargo &unitl)
  {
    uldtype= unitl.getuldtype();
    abbrev= unitl.getabbrev();
    uldid= unitl.getuldid();
    aircraft= unitl.getaircraft();
    weight= unitl.getweight();
    destination= unitl.getdestination();
  }
  Boeing737(const string &uldtype,
            const string &abbrev,
            const string &uldid,
            const int &aircraft,
            const double &weight,
            const string &destination)
  : Cargo(uldtype,abbrev,uldid,aircraft,weight,destination)
  {
    Cargo(uldtype,abbrev,uldid,aircraft,weight,destination);
  }
  ~Boeing737()
  {}
  bool maxweight(int total) const;

};


class Boeing767: public Cargo
{
private:
public:
  Boeing767() : Cargo()
  {
    Cargo();
  }
  Boeing767(const Cargo &unitl)
  {
    uldtype= unitl.getuldtype();
    abbrev= unitl.getabbrev();
    uldid= unitl.getuldid();
    aircraft= unitl.getaircraft();
    weight= unitl.getweight();
    destination= unitl.getdestination();
  }
  Boeing767(const string &uldtype, const string &abbrev, const string &uldid,
  const int &aircraft, const double &weight, const string &destination)
  :Cargo(uldtype,abbrev,uldid,aircraft,weight,destination)
  {
    Cargo(uldtype,abbrev,uldid,aircraft,weight,destination);
  }
  ~Boeing767()
  {
  }
  bool maxweight(int total) const;
};



void load737(Boeing737 *head,Boeing737 *last, const string type, const string abrv,
const string id, const int craft,
const double wt, const string dest, double &total737wt);
void load767(Boeing767 *head,Boeing767 *last, const string type, const string abrv,
const string id, const int craft,
const double wt, const string dest, double &total767wt);
void input();


int main()
{
  input();
  return 0;
}


Cargo::Cargo()
{
  uldtype = "XXX";
  abbrev = " ";
  uldid = "xxxxxIB";
  aircraft = 700;
  weight = 0.0;
  destination = "NONE";
}


Cargo::Cargo(const string &uld,
             const string &abrv,
             const string &id,
             const int &craft,
             const double &wt,
             const string &dest)
{
  uldtype = uld;
  abbrev = abrv;
  uldid = id;
  aircraft = craft;
  weight = wt;
  destination = dest;
}

Cargo::Cargo(const Cargo &unitl)
{
  uldtype= unitl.uldtype;
  abbrev= unitl.abbrev;
  uldid= unitl.uldid;
  aircraft= unitl.aircraft;
  weight= unitl.weight;
  destination= unitl.destination;
}

Cargo::~Cargo()
{
  //cout<< "Cargo destructor called\n\n";
}

void Cargo::setuldtype(string type)
{
  uldtype = type;
}
void Cargo::setabbrev(string abrv)
{
  abbrev = abrv;
}
void Cargo::setuldid(string id)
{
  uldid = id;
}
void Cargo::setaircraft(int air)
{
  aircraft = air;
}
void Cargo::setweight(double wt)
{
  weight = wt;
}
void Cargo::setdestination(string dest)
{
  destination = dest;
}
string Cargo::getuldtype() const
{
  return uldtype;
}
string Cargo::getabbrev() const
{
  return abbrev;
}
int Cargo::getaircraft() const
{
  return aircraft;
}
double Cargo::getweight() const
{
  return weight;
}
string Cargo::getuldid() const
{
  return uldid;
}
string Cargo::getdestination() const
{
  return destination;
}

bool Boeing737::maxweight(int total) const
{
  return total< maxload737;
}

bool Boeing767::maxweight(int total) const
{
  return total< maxload767;
}

void load737(vector<Boeing737> &vect,
             const string type,
             const string abrv,
             const string id,
             const int craft,
             const double wt,
             const string dest,
             double &total737wt)
{
  bool f=0;
  for(int i=0;i<8;i++)
  {
    if(abrv==plane767[i])
    {
      f=1;
      break;
    }
  }
  if(f)
  {
    cout << "The " << abrv << " " << type << " is not compatible with the 737 aircraft\n" << endl;
    return;
  }
  total737wt = total737wt + wt;
  Boeing737 *ldunit=new Boeing737(type,abrv,id,craft,wt,dest);
  if(ldunit->maxweight(total737wt))
  {
    vect.push_back((*ldunit));
  }
  else
  {
    total737wt = total737wt - wt;
    cout << "Unit " << type <<" was not added due to weight restrictions\n" << endl;
  }
}

void load767(vector<Boeing767> &vect,
             const string type,
             const string abrv,
             const string id,
             const int craft,
             const double wt,
             const string dest,
             double &total767wt)
{
  bool f=0;
  for(int i=0;i<8;i++)
  {
    if(abrv==plane767[i])
    {
      f=1;
      break;
    }
  }
  if(!f)
  {
    cout << "The " << abrv << " " << type << " is not compatible with the 737 aircraft\n" << endl;
    return;
  }
  total767wt = total767wt + wt;
  //cout<< "total767wt is " << total767wt<< endl;
  Boeing767 *ldunit=new Boeing767(type,abrv,id,craft,wt,dest);
  if(ldunit->maxweight(total767wt))
  {
    vect.push_back((*ldunit));
  }
  else
  {
    total767wt = total767wt - wt;
    cout << "Unit " << type << " not added due to weight restrictions\n" << endl;
  }
}

void Cargo::output()
{
  cout<< fixed<< showpoint<< setprecision(2);
  cout<< setw(19)<< "Unit load type: "<< uldtype << endl;
  cout<< setw(19)<< "Unit abbreviation: "<< abbrev << endl;
  cout<< setw(19)<< "Unit identifier: "<< uldid << endl;
  cout<< setw(19)<< "Aircraft type: "<< aircraft <<endl;
  cout<< setw(19)<< "Unit weight: "<< weight << " pounds"<<endl;
  cout<< setw(19)<< "Destination code: "<< destination <<endl;
  cout << "-----------------------\n" << endl;
}

void input()
{
  string type;
  string abrv;
  string id;
  int air;
  double wt;
  string dest;
  double total737wt = 0.0;
  double total767wt = 0.0;
  vector<Boeing737> vect737; //vector for Boeing737
  vector<Boeing767> vect767; //vector for Boeing767
  ifstream inputFile;
  inputFile.open("lab5data.txt");
  if(!inputFile)
  {
    std::cerr << "Error opening the file" << endl;
    exit(100);
  }
  while(inputFile >> type)
  {
      inputFile >> abrv;
      inputFile >> id;
      inputFile >> air;
      inputFile >> wt;
      inputFile >> dest;
      if (air == 737)
      {
        load737(vect737,type,abrv,id,air,wt,dest,total737wt);
      }
      else if (air == 767)
      {
        load767(vect767,type,abrv,id,air,wt,dest,total767wt);
      }
      else
      {
          cout << "Unknown aircraft type\n" << endl;
      }
  }
  inputFile.close();
  for(int i=0;i<vect737.size();i++)
  {
    cout << "Unit " << i+1 << endl;
    vect737[i].output();
  }
  cout<<endl;
  for(int i=0;i<vect767.size();i++)
  {
    cout << "Unit " << i+1 <<endl;
    vect767[i].output();
  }
}

```
