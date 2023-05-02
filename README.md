# Hotelbooking_sys
class Room:
    def __init__(self, room_number, room_type, price_per_night, is_available=True):
        self.room_number = room_number
        self.room_type = room_type
        self.price_per_night = price_per_night
        self.is_available = is_available

class Hotel:
    def __init__(self, hotel_name, rooms):
        self.hotel_name = hotel_name
        self.rooms = rooms
        self.bookings = []

   def display_rooms(self):
        print(f"{self.hotel_name} Room List:")
        print("Room Number\tRoom Type\tPrice Per Night\tAvailability")
        for room in self.rooms:
            print(f"{room.room_number}\t\t{room.room_type}\t\t{room.price_per_night}\t\t{'Available' if room.is_available else 'Booked'}")

   def search_available_rooms(self, checkin_date, checkout_date):
        available_rooms = []
        for room in self.rooms:
            if room.is_available:
                available_rooms.append(room)
        for booking in self.bookings:
            if checkin_date < booking.checkout_date and checkout_date > booking.checkin_date:
                available_rooms = [room for room in available_rooms if room not in booking.rooms]
        return available_rooms

   def create_booking(self, guest_name, checkin_date, checkout_date, rooms):
        booking = Booking(guest_name, checkin_date, checkout_date, rooms)
        self.bookings.append(booking)
        for room in rooms:
            room.is_available = False
        return booking

class Booking:
    def __init__(self, guest_name, checkin_date, checkout_date, rooms):
        self.guest_name = guest_name
        self.checkin_date = checkin_date
        self.checkout_date = checkout_date
        self.rooms = rooms

   def get_total_price(self):
        total_price = 0
        for room in self.rooms:
            total_price += room.price_per_night
        return total_price * (self.checkout_date - self.checkin_date).days

if __name__ == "__main__":
    # Create a hotel with some rooms
    rooms = [
        Room(101, "Single", 50),
        Room(102, "Single", 50),
        Room(103, "Double", 75),
        Room(104, "Double", 75),
        Room(105, "Suite", 100),
    ]
    hotel = Hotel("My Hotel", rooms)

    # Display the hotel's room list
   hotel.display_rooms()

    # Search for available rooms for a given date range
   from datetime import date, timedelta
   checkin_date = date.today() + timedelta(days=7)
   checkout_date = checkin_date + timedelta(days=3)
   available_rooms = hotel.search_available_rooms(checkin_date, checkout_date)
   print(f"Available Rooms for {checkin_date} - {checkout_date}:")
   for room in available_rooms:
       print(f"Room {room.room_number} ({room.room_type}): {room.price_per_night}/night")

    # Create a booking
   rooms_to_book = [room for room in available_rooms[:2]]
   booking = hotel.create_booking("John Smith", checkin_date, checkout_date, rooms_to_book)
   print("Booking created:")
   print(f"Guest: {booking.guest_name}")
   print(f"Checkin Date: {booking.checkin_date}")
   print(f"Checkout Date: {booking.checkout_date}")
   print("Rooms Booked:")
   for room in booking.rooms:
       print(f"- Room {room.room
