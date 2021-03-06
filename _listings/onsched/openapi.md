swagger: "2.0"
x-collection-name: OnSched
x-complete: 1
info:
  title: OnSched API
  description: build-secure-and-scalable-custom-apps-for-online-booking--our-flexible-api-provides-many-options-for-availability-and-booking--take-the-api-for-a-test-drive--just-click-on-the-authorize-button-above-and-authenticate---you-can-access-our-demo-company-profile-if-you-are-not-a-customer-or-your-own-profile-by-using-your-assigned-clientid-and-secret---------------------
  termsOfService: None
  contact:
    name: OnSched.com
    url: https://onsched.com
    email: info@onsched.com
  version: 1.0.0
host: api.onsched.com
basePath: /
schemes:
- http
produces:
- application/json
consumes:
- application/json
paths:
  /consumer/v1/appointments:
    get:
      summary: Returns a list of appointments.
      description: "The results are returned in pages. Use the offset and limit parameters
        to control the page start and size. Default offset is 0, and limit is 20.\r\nUse
        the other query parameters to optionally filter the results list."
      operationId: ConsumerV1AppointmentsGet
      x-api-path-slug: consumerv1appointments-get
      parameters:
      - in: query
        name: customerId
        description: Filter appointments by customer
      - in: query
        name: email
        description: Filter appointments by email address
      - in: query
        name: limit
        description: Page limit, default 20
      - in: query
        name: locationId
        description: The id of the business location
      - in: query
        name: offset
        description: Starting row of page, default 0
      - in: query
        name: resourceId
        description: Filter appointments by resource
      - in: query
        name: serviceId
        description: Filter appointments by service
      - in: query
        name: status
        description: 'Filter appointments by status: IN, BK, CN, RE, RS'
      responses:
        200:
          description: OK
      tags:
      - Appointments
    post:
      summary: Returns an appointment object
      description: "This end point creates a new appointment in an Initial \"IN\"
        status. \r\n\r\nA valid serviceId is required. Use GET consumer/v1/services
        to retrieve a list of your services.\r\n\r\nA valid resourceId is required
        if your calendar is a resource based calendar. Use consumer/v1/resources to
        retrieve a list of your resources.\r\n\r\nStartDateTime and EndDateTime are
        required. Use the ISO 8601 format for DateTime Timezone. e.g. 2016-10-30T9:00:00-5:00"
      operationId: ConsumerV1AppointmentsPost
      x-api-path-slug: consumerv1appointments-post
      parameters:
      - in: body
        name: appointmentIM
        schema:
          $ref: '#/definitions/holder'
      responses:
        200:
          description: OK
      tags:
      - Appointments
  /consumer/v1/appointments/{id}:
    get:
      summary: Returns an appointment object.
      description: "The result returned is a single appointment object. A valid id
        is required to find the appointment. \r\n\r\nFind appointment id's using the
        GET consumer/v1/appointments end point."
      operationId: ConsumerV1AppointmentsByIdGet
      x-api-path-slug: consumerv1appointmentsid-get
      parameters:
      - in: path
        name: id
        description: The id of the appointment object
      responses:
        200:
          description: OK
      tags:
      - Appointments
    delete:
      summary: Returns an appointment object
      description: "This end point deletes a booking. Only appointments in a \"IN\"
        initial status can be deleted.\r\nPast dated appointments cannot be cancelled.\r\n\r\nA
        valid appointment id is required. Use the appointmentId returned from GET
        /consumer/v1/appointments"
      operationId: ConsumerV1AppointmentsByIdDelete
      x-api-path-slug: consumerv1appointmentsid-delete
      parameters:
      - in: path
        name: id
      responses:
        200:
          description: OK
      tags:
      - Appointments
  /consumer/v1/appointments/{id}/book:
    put:
      summary: Returns an appointment object
      description: "This end point completes a new booking. Only appointments in the
        \"IN\" initial status can be booked.\r\nby saving all the relevant details
        of the booking.\r\n\r\nA valid appointment id is required. Use the appointmentId
        returned from POST /consumer/v1/appointments\r\n\r\nTo update appointment
        custom field values, use the GET /consumer/v1/appointments/customfields information\r\nto
        understand your definitions of custom fields and what key and values to update."
      operationId: ConsumerV1AppointmentsByIdBookPut
      x-api-path-slug: consumerv1appointmentsidbook-put
      parameters:
      - in: body
        name: appointmentBookModel
        schema:
          $ref: '#/definitions/holder'
      - in: path
        name: id
      responses:
        200:
          description: OK
      tags:
      - Appointments
      - Book
  /consumer/v1/appointments/{id}/cancel:
    put:
      summary: Returns an appointment object
      description: "This end point cancels a booking or reservation. Only appointments
        in a \"BK\" booked, or \"RS\" reserved status can be cancelled.\r\nPast dated
        appointments cannot be cancelled.\r\n\r\nA valid appointment id is required.
        Use the appointmentId returned from POST /consumer/v1/appointments"
      operationId: ConsumerV1AppointmentsByIdCancelPut
      x-api-path-slug: consumerv1appointmentsidcancel-put
      parameters:
      - in: path
        name: id
      responses:
        200:
          description: OK
      tags:
      - Appointments
      - Cancel
  /consumer/v1/appointments/{id}/reschedule:
    put:
      summary: Returns an appointment object
      description: "This end point reschedules a booking. Only appointments in a \"BK\"
        booked status can be rescheduled.\r\nPast dated appointments cannot be cancelled.\r\n\r\nA
        valid appointment id is required. Use the appointmentId returned from GET
        /consumer/v1/appointments.\r\n\r\nThe serviceId and resourceId are optional.
        If you want your users to change the resource or service on a reschedule\r\nyou
        will have to use the regular availability endpoint rather than the reschedule
        availability end point.\r\n\r\nUse the GET /consumer/v1/availability/{id}/reschedule
        endpoint to display a list of available times\r\nfor the end user to choose
        from to reschedule the original appointment."
      operationId: ConsumerV1AppointmentsByIdReschedulePut
      x-api-path-slug: consumerv1appointmentsidreschedule-put
      parameters:
      - in: body
        name: appointmentRM
        schema:
          $ref: '#/definitions/holder'
      - in: path
        name: id
        description: appointment id
      responses:
        200:
          description: OK
      tags:
      - Appointments
      - Reschedule
  /consumer/v1/appointments/customfields:
    get:
      summary: Returns a list of appointment custom field definitions
      description: "This end point returns your Appointment custom field definitions.\r\n\r\nAppointment
        custom fields are different than Customer custom fields. Appointment custom
        fields are\r\nstored with each appointment. They are used when the information
        collected during the booking is specific\r\nto a particular visit.\r\n\r\nUse
        the field, and type to determine how to update field values\r\nin PUT /consumer/v1/appointments/{id}/book"
      operationId: ConsumerV1AppointmentsCustomfieldsGet
      x-api-path-slug: consumerv1appointmentscustomfields-get
      parameters:
      - in: query
        name: locationId
        description: The id of the business location
      responses:
        200:
          description: OK
      tags:
      - Appointments
      - Customfields
  /consumer/v1/appointments/bookingfields:
    get:
      summary: ""
      description: ""
      operationId: ConsumerV1AppointmentsBookingfieldsGet
      x-api-path-slug: consumerv1appointmentsbookingfields-get
      parameters:
      - in: query
        name: locationId
      responses:
        200:
          description: OK
      tags:
      - Appointments
      - Bookingfields
  /consumer/v1/availability/{appointmentId}/reschedule:
    get:
      summary: Returns a list of available times.
      description: "This end point is used to find availability for the purpose of
        rescheduling an appointment.\r\nAvailability defaults to the serviceId, resourceId
        and timezone from the original appointment.\r\nAfter choosing from the availability,
        you can call the appointment reschedule endpoint."
      operationId: ConsumerV1AvailabilityByAppointmentIdRescheduleGet
      x-api-path-slug: consumerv1availabilityappointmentidreschedule-get
      parameters:
      - in: path
        name: appointmentId
        description: Appointment Id of the original appointment being rescheduled
      - in: query
        name: tzOffset
        description: Request timezone offset to view availability
      responses:
        200:
          description: OK
      tags:
      - Availability
      - AppointmentId
      - Reschedule