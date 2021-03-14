#### schema

```ruby
ActiveRecord::Schema.define(version: 20200630185941) do

  create_table "appointments", force: :cascade do |t|
    t.datetime "appointment_datetime"
      #datetime and date allow have different restrictions on what you can passin as and arrgument
    t.integer  "patient_id"
    t.integer  "doctor_id"
    t.datetime "created_at",           null: false
    t.datetime "updated_at",           null: false
    t.index ["doctor_id"], name: "index_appointments_on_doctor_id"
    t.index ["patient_id"], name: "index_appointments_on_patient_id"
  end

  create_table "doctors", force: :cascade do |t|
    t.string   "name"
    t.string   "department"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "patients", force: :cascade do |t|
    t.string   "name"
    t.integer  "age"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

end
```

#### models

```ruby
#app/models/appointment
class Appointment < ApplicationRecord
  belongs_to :patient
  belongs_to :doctor

  def time
    self.appointment_datetime.strftime("%B %d, %Y at %H:%M")
  end
end

#app/models/doctor
class Doctor < ApplicationRecord
  has_many :appointments
  has_many :patients, through: :appointments 

  def time
    self.appointments.appointment_datetime.strftime("%B %d, %Y at %H")
  end
end

#app/models/patient
class Patient < ApplicationRecord
  has_many :appointments
  has_many :doctors, through: :appointments
end

```

#### controllers

```ruby
#app/controllers/appointments
class AppointmentsController < ApplicationController
  def show
    @appointment = Appointment.find(params[:id])
  end
end

---------------------------------------------------------------
#app/controllers/doctor
class DoctorsController < ApplicationController
  def index
    @doctors = Doctor.all
  end

  def new
    @doctor = Doctor.new
  end

  def create
    @doctor = Doctor.new(doctor_params)

    if @doctor.save
      redirect_to doctors_path
    else
      render :new 
    end
  end

  def show
    @doctor = Doctor.find(params[:id])
  end

  def edit
    @doctor = Doctor.find(params[:id])
  end

  def update
    @doctor = Doctor.update(doctor_params)

    if @doctor.save
      redirect_to @doctor
    else
      render :edit
    end
  end

  def destroy
    @doctor = Doctor.find(params[:id])
    @dector.destroy
    redirect_to doctors_path
  end

  private

  def doctor_params
    params.require(:doctor).permit(
      :name,
      :department,
      appointments_attributes: [
        :appointment_datetime
      ]
    ) 
  end
end
 
---------------------------------------------------------------
#app/controllers/patients
class PatientsController < ApplicationController
  def index
    @patients = Patient.all
  end

  def new
    @patient = Patient.new
  end
  
  def create
    @patient = Patient.create(patient_params)

    if @patient.save
      redirect_to patients_path
    else
      render :new
    end
  end

  def show
    @patient = Patient.find(params[:id])
  end

  def edit
    @patient = Patient.find(params[:id])
  end

  def update
    @patient = Patient.find(params[:id])
    @patient.update(patient_params)

    if @patient.save
      redirect_to patient_pathe(@patient)
    else
      render :edit
    end
  end

  def destroy
    @patient = Patient.find(params[:id])
    @patient.destroy

    redirect_to patients_path
  end

  private
  def patients_params
    params.require(:patient).permit(
      :name,
      :age,
      appointments_attributes: [
        :appointment_datetime
      ],
    )
  end
end

```

```erb
<h1><%= link_to @appointment.patient.name, patient_path(@appointment.patient) %></h1>
<h2>with <%= link_to @appointment.doctor.name, doctor_path(@appointment.doctor) %></h2>
<h2>on <%= @appointment.time %></h2>
```

```erb
<h1><%= @doctor.name %></h1>
<ul>
<% @doctor.appointments.each do |a|%>

<li><%= link_to a.patient.name, patient_path(a.patient) %> <%= a.time %></li>
<% end %>
</ul>
```

```erb
<h1><%= @patient.name %></h1>
<ul>
<% @patient.appointments.each do |a|%>

<li><%= link_to a.doctor.name, doctor_path(a.doctor) %> <%= a.time %></li>
<% end %>
</ul>
```



